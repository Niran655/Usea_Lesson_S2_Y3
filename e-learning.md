# HR Training and Learning Features

This document describes the five entries in the HR sidebar section **បណ្តុះបណ្តាល និង ប្រឡង** (Training and Exams), shown in the supplied screenshot.

## Sidebar Pages

| Sidebar label | Meaning | Frontend route | Main page component | Main data |
| --- | --- | --- | --- | --- |
| វគ្គបណ្តុះបណ្តាល | Training courses | `/hr/trainings` | `TrainingsPage.jsx` | `trainings` |
| អ្នកចូលរួមបណ្តុះបណ្តាល | Training participants | `/hr/training-participants` | `TrainingParticipantsPage.jsx` | `training_participants` |
| គ្រប់គ្រងប្រឡងបុគ្គលិក | Employee exam management | `/hr/exams` | `ExamsPage.jsx` | Exam and question tables listed below |
| ការសិក្សាតាមអេឡិចត្រូនិក | E-learning management | `/hr/elearning` | `ElearningPage.jsx` | `elearning_lessons` |
| មេរៀនរបស់ខ្ញុំ | My lessons | `/hr/my-elearning` | `MyElearningPage.jsx` | Published rows from `elearning_lessons` |

The sidebar declarations are in `frontend/src/components/Sidebar.jsx`. The routes are registered in `frontend/src/dashboard/HRSystem.jsx` and mapped from menu IDs in `frontend/src/js/routeConfig.js`.

## Training Courses

The courses page is a reusable HR resource page configured in `frontend/src/dashboard/hr/pages/TrainingsPage.jsx`. It creates, lists, searches, edits, and deletes course records through the HR resource API.

### Table: `trainings`

Created by `backend/database/migrations/2026_07_14_150000_create_company_hr_extension_tables.php`; additional columns are added in `backend/database/migrations/2026_07_14_170000_extend_hr_full_system_fields.php`.

| Column | Purpose |
| --- | --- |
| `id` | Course identifier |
| `title` | Required course name |
| `training_type` | Orientation, technical, soft skill, compliance, leadership, or other |
| `provider`, `trainer` | Provider and instructor |
| `start_date`, `end_date` | Course schedule; end date cannot be before start date |
| `location` | Where the course is held |
| `capacity` | Maximum participant count |
| `target_audience` | Intended employee group |
| `certificate_available` | Whether the course offers a certificate |
| `evaluation_method` | How course outcomes are evaluated |
| `budget` | Course budget |
| `status` | `planned`, `ongoing`, `completed`, or `cancelled` |
| `description` | Course details |
| `created_at`, `updated_at` | Laravel timestamps |

The model is `backend/app/Models/HrTraining.php`. A training has many participant records. The page's summary counts courses by planned, ongoing, and completed status.

## Training Participants

The participants page is configured in `frontend/src/dashboard/hr/pages/TrainingParticipantsPage.jsx`. It records an employee's registration and outcome for one course.

### Table: `training_participants`

Created in the company HR extension migration, with registration and feedback columns added by the full-system-fields migration.

| Column | Purpose |
| --- | --- |
| `id` | Participant-record identifier |
| `training_id` | Required reference to `trainings.id`; deleting the course cascades to its participant records |
| `employee_id` | Optional reference to `employees.id`; deleting the employee sets this value to null |
| `registered_at` | Registration date |
| `attendance_status` | `registered`, `attended`, `absent`, or `completed` |
| `score` | Optional score from 0 to 100 |
| `result` | `passed`, `failed`, or `incomplete` |
| `completed_at` | Completion date |
| `certificate_no`, `certificate_issued_at` | Certificate number and issue date |
| `feedback` | Participant feedback |
| `notes` | Internal notes |
| `created_at`, `updated_at` | Laravel timestamps |

Each course/employee pair is unique, so the same employee cannot be added to the same course twice. The model relationships are in `backend/app/Models/HrTraining.php` and `backend/app/Models/HrTrainingParticipant.php`.

## Employee Exams

The exam page is a larger workflow in `frontend/src/dashboard/hr/pages/ExamsPage.jsx`, backed by `backend/app/Http/Controllers/HrExamController.php`. Its tabs cover question bank, exams, assignment, taking an exam, and results.

| Table | Purpose and important relationships |
| --- | --- |
| `question_categories` | Named categories for organizing questions; `name` is unique |
| `questions` | Question text, type, difficulty, points, explanation, metadata, status; optional `category_id` links to `question_categories` |
| `question_options` | Answer options for a question, including whether each option is correct; deleting the question cascades to its options |
| `exams` | Exam configuration: title, time limit, maximum attempts, open/close times, passing score, randomization, feedback timing, anti-cheat options, and status |
| `exam_questions` | Join table connecting exams and questions, with per-exam points and order; each exam/question pair is unique |
| `exam_assignments` | Assigns an exam to an employee and/or user, with status, assignment time, and due date; duplicate exam/employee or exam/user pairs are prevented |
| `exam_attempts` | One employee/user attempt, linked to its exam and optionally its assignment; stores start/submit/expiry times, score, percentage, pass state, tab switches, and question order |
| `exam_answers` | One answer per question per attempt; stores selected option or text answer, correctness, and awarded points |

The exam tables are created in `backend/database/migrations/2026_08_17_100000_create_hr_exam_tables.php`. Later migrations add `max_attempts` and optional `user_id` links to assignments and attempts. Foreign keys generally cascade when their owning exam, question, assignment, or attempt is deleted.

### Exam workflow

1. Create question categories, then add questions and answer options.
2. Create an exam and attach questions through `exam_questions`.
3. Publish the exam and assign it through `exam_assignments` with an optional due date.
4. The assigned employee starts an attempt. The system records the attempt and submitted answers.
5. Results are stored on `exam_attempts`; individual answer marks are stored in `exam_answers`.

## E-Learning Lessons

The manager-facing page is `frontend/src/dashboard/hr/pages/ElearningPage.jsx`. It creates and manages lesson content, chooses individual employees and/or departments as recipients, sets a due date, and selects a status. Lessons can use a video URL or an uploaded video file.

### Table: `elearning_lessons`

Created by `backend/database/migrations/2026_10_04_000002_create_elearning_lessons.php`; `video_file` is added by `backend/database/migrations/2026_10_04_000003_add_video_file_to_elearning_lessons.php`.

| Column | Purpose |
| --- | --- |
| `id` | Lesson identifier |
| `title` | Lesson title |
| `content` | Optional lesson text/content |
| `video_url` | Optional external video URL |
| `video_file` | Optional path for an uploaded video in public storage |
| `employee_ids` | JSON array of `employees.id` values assigned to the lesson |
| `departments` | JSON array of department names assigned to the lesson |
| `status` | `draft`, `published`, or `archived` |
| `due_date` | Optional completion deadline |
| `created_at`, `updated_at` | Laravel timestamps |

The model and JSON/date casts are in `backend/app/Models/HrElearningLesson.php`. Recipients are stored as JSON arrays rather than relational pivot tables. Uploaded video files are stored on Laravel's public storage disk.

### Employee lesson workflow

`MyElearningPage.jsx` is read-only. Its resource endpoint asks the backend to find the signed-in user's employee record, then returns only published lessons assigned directly to that employee or to the employee's department. The employee can open lesson content and play or open its video. The current schema does not contain a lesson completion/progress table, so opening a lesson does not record completion.

## API and Permissions

The frontend uses the HR route prefix declared in `backend/routes/api.php`.

| Feature | API routes |
| --- | --- |
| Trainings | `/api/hr/trainings` and `/api/hr/trainings/{id}` |
| Training participants | `/api/hr/training-participants` and `/api/hr/training-participants/{id}` |
| E-learning management | `/api/hr/elearning-lessons` and `/api/hr/elearning-lessons/{id}` |
| My lessons | `/api/hr/my-elearning-lessons` |
| Exam management | `/api/hr/exam-categories`, `/api/hr/exam-questions`, `/api/hr/exams`, `/api/hr/exam-assignments`, and `/api/hr/exam-results` |
| Employee exam access | `/api/hr/my-exam-assignments`, `/api/hr/my-exam-results`, `/api/hr/exam-assignments/{id}/start`, and `/api/hr/exam-attempts/{id}/submit` |

Training and participant endpoints use the generic resource handlers in `HrController`. E-learning lessons use those handlers with dedicated validation and the same CRUD path pattern. Exam endpoints use `HrExamController`. E-learning administration routes check `hr.elearning.view/create/edit/delete`; exam-management routes check `hr.exams.manage`. Employee lesson and exam routes provide the signed-in employee's personal view.

## Main Source Files

- Sidebar and section labels: `frontend/src/components/Sidebar.jsx`
- Frontend HR routes: `frontend/src/dashboard/HRSystem.jsx`, `frontend/src/js/routeConfig.js`
- Course and participant pages: `frontend/src/dashboard/hr/pages/TrainingsPage.jsx`, `frontend/src/dashboard/hr/pages/TrainingParticipantsPage.jsx`
- Exam page: `frontend/src/dashboard/hr/pages/ExamsPage.jsx`
- E-learning manager and employee pages: `frontend/src/dashboard/hr/pages/ElearningPage.jsx`, `frontend/src/dashboard/hr/pages/MyElearningPage.jsx`
- Shared HR CRUD API: `backend/app/Http/Controllers/HrController.php`
- Exam API: `backend/app/Http/Controllers/HrExamController.php`
- API route declarations: `backend/routes/api.php`
- Training/e-learning models: `backend/app/Models/HrTraining.php`, `backend/app/Models/HrTrainingParticipant.php`, `backend/app/Models/HrElearningLesson.php`
- Training migrations: `backend/database/migrations/2026_07_14_150000_create_company_hr_extension_tables.php`, `backend/database/migrations/2026_07_14_170000_extend_hr_full_system_fields.php`
- Exam migration: `backend/database/migrations/2026_08_17_100000_create_hr_exam_tables.php`
- E-learning migrations: `backend/database/migrations/2026_10_04_000002_create_elearning_lessons.php`, `backend/database/migrations/2026_10_04_000003_add_video_file_to_elearning_lessons.php`
