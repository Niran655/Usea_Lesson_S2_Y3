នេះជាខ្លឹមសារលម្អិតបំផុតនៃ **Chapter 6: Work with Menus and the Action Bar** (បង្រៀនដោយ៖ អ្នកគ្រូ សា សុខងីម ឆ្នាំសិក្សា ២០២៤) ដែលត្រូវបានពង្រីក និងបកស្រាយគ្រប់ជំហាន ទាំងផ្នែកទ្រឹស្តី ស្ថាបត្យកម្ម កូដ XML និង Java៖

---

# ផ្នែកទី ១៖ ព័ត៌មានទូទៅ និងលទ្ធផលរំពឹងទុក (Course Overview & LLOs)

### ១. លទ្ធផលរំពឹងទុកនៃការសិក្សា (Lesson Learning Outcomes - LLOs)
* **ពន្យល់ពី Menu៖** អាចពន្យល់ពីនិយមន័យ និងការប្រើប្រាស់ Menu ក្នុងប្រព័ន្ធ Android បានយ៉ាងក្បោះក្បាយ।
* **បែងចែកប្រភេទ Menu៖** កំណត់សម្គាល់ និងបែងចែកប្រភេទផ្សេងគ្នានៃ Menu ដូចជា **Options Menu**, **Context Menu**, និង **Popup Menu** បានយ៉ាងច្បាស់លាស់।
* **ការបង្កើត Menu៖** បង្កើត Menu តាមទម្រង់ XML និង Java បានយ៉ាងត្រឹមត្រូវ।
* **Event Handling៖** ប្រើប្រាស់ Event Handling ជាមួយ Items របស់ Menu បានយ៉ាងត្រឹមត្រូវ।
* **ការអនុវត្ត App ជាក់ស្តែង៖** អនុវត្តបង្កើត App ដែលមាន Menu ដូចជា *Main Screen with Option Bar* និង *ToDo List App with Context and Option Menu*।
* **ការសិក្សាស្រាវជ្រាវបន្ថែម៖** អាចសិក្សាបន្ថែមអំពី Menu ដែលមាន Sub Menu ឬបណ្ណាល័យ (Libraries) ផ្សេងៗទៀត।

---

# ផ្នែកទី ២៖ ស្ថាបត្យកម្មគ្រឹះនៃ Menu និង App Bar

### ១. អ្វីទៅជា Menu? (What is Menu?)
**Menu** គឺជាធាតុផ្សំ User Interface (UI) ដ៏សំខាន់មួយ ដែលផ្តល់នូវជម្រើសសកម្មភាព (Action Options) សម្រាប់ View ឬ Activity ជាក់លាក់।

### ២. ប្រភេទទាំង ៣ នៃ Menu (Types of Menu)
1. **Option menu and app bar**
2. **Context Menu and Contextual action Mode**
3. **Pop Up menu**

### ៣. App Bar / Action Bar
* **និយមន័យ៖** Action Bar គឺជា របារប្រៀបដូចជាបន្ទះផ្ទេកនៃ Icons (Horizontal strip of icons) ដែលបង្ហាញនៅផ្នែកខាងលើបង្អស់នៃ Activity ភាគច្រើនក្នុង Android। វាមានផ្ទុក Title របស់ Activity និង Action Items មួយចំនួន।
* **ការកំណត់ Theme សម្រាប់ Action Bar ក្នុង XML (`res/values/themes.xml`)៖**
```xml
<!-- Base application theme. -->
<style name="Theme.ToDoList" parent="Theme.MaterialComponents.DayNight.DarkActionBar">
    <!-- Primary brand color. -->
    <item name="colorPrimary">@color/purple_500</item>
    <item name="colorPrimaryVariant">@color/purple_700</item>
    <item name="colorOnPrimary">@color/white</item>
    <!-- Secondary brand color. -->
    <item name="colorSecondary">@color/teal_200</item>
    <item name="colorSecondaryVariant">@color/teal_700</item>
    <item name="colorOnSecondary">@color/black</item>
    <!-- Status bar color. -->
    <item name="android:statusBarColor" tools:targetApi="l">?attr/colorPrimaryVariant</item>
</style>
```

---

# ផ្នែកទី ៣៖ ការសិក្សាស្វែងយល់លម្អិតពី Menu នីមួយៗ

## ១. Options Menu និង App Bar
### ក. និយមន័យ និងគោលបំណង
* **Options Menu** គឺជាបណ្តុំ Menu Items ស្នូល/ចម្បងសម្រាប់ Activity। វាជាកន្លែងដែលត្រូវដាក់សកម្មភាពដែលមានឥទ្ធិពលជាសកល (Global Impact) លើ App ដូចជា **"Search"**, **"Compose email"**, និង **"Settings"**।
* មុខងារនេះត្រូវបានគាំទ្រចាប់ពី **Android 3.0 (API level 11)** ឬខ្ពស់ជាងនេះ (ឧទាហរណ៍៖ Options menu ក្នុង Browser ឬ Google Play Movies app)।

### ខ. ជំហានទាំង ៣ ក្នុងការបង្កើត Options Menu
1. បង្កើតឯកសារ `option_menu.xml` នៅក្នុងថត `/res/menu`।
2. ធ្វើការ Override វិធីសាស្ត្រ `onCreateOptionsMenu()` ក្នុង Activity និង Load ឯកសារ Menu Resource ដោយប្រើ `MenuInflater.inflate()`।
3. ចាប់ព្រឹត្តិការណ៍ Click Event លើ Options Menu តាមរយៈវិធីសាស្ត្រ `onOptionsItemSelected()`।

### គ. ការបង្កើត XML Resource (`/res/menu/option_game.xml`)
* ឯកសារ XML ត្រូវបង្កើតឡើងក្នុងថត `res/menu/`। Tag មេត្រូវតែជា `<menu>` ដែលជា Container សម្រាប់ផ្ទុក `<item>` និង `<group>`।

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/new_game"
          android:icon="@drawable/ic_new_game"
          android:title="@string/new_game"
          android:showAsAction="ifRoom"/>

    <item android:id="@+id/help"
          android:icon="@drawable/ic_help"
          android:title="@string/help" />
</menu>
```

* **ការបកស្រាយលម្អិតលើ XML Attributes៖**
  * `android:id`: ជា Resource ID តែមួយគត់សម្រាប់ Item ដែលអនុញ្ញាតឱ្យ App ចំណាំបាននៅពេលអ្នកប្រើប្រាស់ជ្រើសរើសវា។
  * `android:icon`: ជា Reference ទៅកាន់ Drawable Resource សម្រាប់ប្រើជារូបតំណាង (Icon) របស់ Item۔
  * `android:title`: ជា Reference ទៅកាន់ String Resource សម្រាប់ប្រើជាចំណងជើង (Title) របស់ Item۔
  * `android:showAsAction`: កំណត់ថាតើពេលណា និងរបៀបណាដែល Item នេះត្រូវបង្ហាញជា Action Item នៅលើ App Bar (ឧទាហរណ៍ `ifRoom` មានន័យថាបង្ហាញជា Icon លើ App Bar បើមានកន្លែងទំនេរ)۔

### ឃ. កូដ Java សម្រាប់ Options Menu (`MainActivity.java`)
* **ការ Load Menu Resource ចូល Activity៖**
```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    MenuInflater inflater = getMenuInflater();
    inflater.inflate(R.menu.option_game, menu); // Load ឯកសារ option_game.xml
    return super.onCreateOptionsMenu(menu);
}
```

* **ការចាប់ព្រឹត្តិការណ៍ Click Event តាមរយះ `onOptionsItemSelected()`៖**
  * វិធីសាស្ត្រនេះបញ្ជូន Parameter ជា `MenuItem` ដែលបានជ្រើសរើស।
  * យើងអាចស្គាល់ Item នោះបានតាមរយៈការហៅ `item.getItemId()` ដែលត្រឡប់មកវិញនូវ Unique ID (កំណត់ក្នុង `android:id` នៃ XML)।

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    // Handle item selection
    switch (item.getItemId()) {
        case R.id.new_game:
            newGame();
            return true;
        case R.id.help:
            showHelp();
            return true;
        default:
            return super.onOptionsItemSelected(item);
    }
}
```

---

## ២. Context Menu (Floating Menu)
### ក. និយមន័យ និងលក្ខណៈ
* **Context Menu** គឺជា Floating Menu ដែលលេចឡើងនៅពេលអ្នកប្រើប្រាស់ធ្វើការ **Long-click** (ចុចសង្កត់ជាប់) លើ Element/View ណាមួយ। វាផ្តល់នូវសកម្មភាព (Context Frame Actions) ដែលប៉ះពាល់ដោយផ្ទាល់ទៅលើ Content ដែលបានជ្រើសរើស।
* **Contextual Action Mode:** បង្ហាញ Action Items ដែលប៉ះពាល់លើ Content ជ្រើសរើស ក្នុងរបារនៅផ្នែកខាងលើអេក្រង់ និងអនុញ្ញាតឱ្យអ្នកប្រើប្រាស់ជ្រើសរើស Items ច្រើនក្នុងពេលតែមួយបាន।

### ខ. Lifecycle និងការបង្កើត Context Menu
1. នៅពេល Context Menu ត្រូវបានបើកជាលើកដំបូង ប្រព័ន្ធ Android នឹងហៅ Callback Method របស់ Activity ឈ្មោះថា `onCreateContextMenu(ContextMenu menu, View v, ContextMenuInfo menuInfo)`।
2. យើងត្រូវ Override វិធីសាស្ត្រនេះ ហើយបំពេញ `Menu` Object ជាមួយ `MenuItem`s ដោយប្រើ `getMenuInflater().inflate()`।
3. យើងអាចកំណត់ Header Title ឱ្យ Context Menu បានតាមរយៈ `menu.setHeaderTitle("Choose Option")`।
4. **ដើម្បីបង្ហាញ Context Menu៖** នៅក្នុង `onCreate()` នៃ Activity ត្រូវចុះឈ្មោះ View ជាមួយ `registerForContextMenu(view)`।

```java
TextView txtContextMenu;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    
    txtContextMenu = findViewById(R.id.txtContextMenu);
    
    // ចុះឈ្មោះ View ដើម្បីបង្ហាញ Context Menu ពេល Long Click
    registerForContextMenu(txtContextMenu); 
}

// Override វិធីសាស្ត្រនេះដើម្បីបង្កើត Menu Items
@Override
public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
    getMenuInflater().inflate(R.menu.context_menu, menu);
    super.onCreateContextMenu(menu, v, menuInfo);
    
    // កំណត់ Header Title សម្រាប់ Menu
    menu.setHeaderTitle("Choose Option");
}
```

### គ. ការចាប់ព្រឹត្តិការណ៍ Selection លើ Context Menu
* នៅពេល Menu Item ត្រូវបានជ្រើសរើសពី Context Menu, Callback Method `onContextItemSelected(MenuItem item)` របស់ Activity នឹងត្រូវហៅ।
* Callback នេះបញ្ជូន `MenuItem` ដែលជ្រើសរើស ដោយយើងអាចទាញយក ID តាមរយៈ `item.getItemId()` ដើម្បីដឹងថាជា Item មួយណា និងអនុវត្តសកម្មភាពសមស្រប।

```java
/* Handles item selections */
@Override
public boolean onContextItemSelected(MenuItem item) {
    switch (item.getItemId()) {
        case MENU_NEW_GAME:
            newGame();
            return true;
        case MENU_QUIT:
            quit();
            return true;
    }
    return false;
}
```

---

## ៣. Popup Menu
### ក. និយមន័យ និងលក្ខណៈចម្បង
* **Popup Menu** បង្ហាញបញ្ជី Items ជាជួរឈរ (Vertical list) ដែលត្រូវបាន **Anchored** (ភ្ជាប់/បោះយុថ្កា) ទៅនឹង View ដែលបានហៅ Menu នោះ।
* **អាកប្បកិរិយា៖** សកម្មភាពក្នុង Popup Menu **មិនគួរប៉ះពាល់ដោយផ្ទាល់ទៅលើ Content ដែលពាក់ព័ន្ធឡើយ** (នោះជាតួនាទីរបស់ Contextual Actions)। Popup Menu គឺសម្រាប់សកម្មភាពបន្ថែម (Extended actions) ដែលទាក់ទងនឹងតំបន់នៃ Content ក្នុង Activity របស់អ្នក।
* **ថ្នាក់មេ៖** Class `android.widget.PopupMenu` គឺជា Subclass ផ្ទាល់របស់ `java.lang.Object`।

### ខ. ជំហានទាំង ៤ ក្នុងការបង្កើត Popup Menu
1. បង្កើតឯកសារ `popup_menu.xml` នៅក្នុង `/res/menu`।
2. បង្កើត Instance ចេញពី Class `PopupMenu` និង Load Menu Resource ដោយប្រើ `MenuInflater.inflate()`।
3. បង្កើត Button នៅក្នុង XML និងប្រកាសក្នុង Java।
4. សរសេរកូដក្នុង Method `onClick` របស់ Button।

### គ. កូដ XML (`/res/menu/popup_menu.xml`)
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/mail"
          android:icon="@drawable/ic_mail"
          android:title="@string/mail" />
    <item android:id="@+id/upload"
          android:icon="@drawable/ic_upload"
          android:title="@string/upload"
          android:showAsAction="ifRoom" />
    <item android:id="@+id/share"
          android:icon="@drawable/ic_share"
          android:title="@string/share" />
</menu>
```

### ឃ. កូដ XML & Java សម្រាប់ធ្វើការងារជាមួយ Popup Menu
* ** Button ក្នុង XML (`activity_main.xml`)៖**
```xml
<Button
    android:id="@+id/btnShow"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Show Popup Menu"
    android:onClick="showPopup" />
```

* ** កូដ Java ពេញលេញក្នុងការបង្កើត និងចាប់ Event លើ Popup Menu (`MainActivity.java`)៖**
```java
Button btn = findViewById(R.id.btnShow);

btn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        // បង្កើត PopupMenu Instance ដោយភ្ជាប់ទៅ view (Button)
        PopupMenu popup = new PopupMenu(MainActivity.this, view);
        MenuInflater inflater = popup.getMenuInflater();
        inflater.inflate(R.menu.item_menu, popup.getMenu());
        popup.show(); // បង្ហាញ Popup Menu

        // កំណត់ Listener សម្រាប់ចាប់ព្រឹត្តិការណ៍ Click លើ Item របស់ Popup Menu
        popup.setOnMenuItemClickListener(new PopupMenu.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(MenuItem item) {
                switch (item.getItemId()) {
                    case R.id.add:
                        AddItem();
                        return true;
                    case R.id.upload:
                        Toast.makeText(MainActivity.this, "Upload Option", Toast.LENGTH_SHORT).show();
                        return true;
                    default:
                        return false;
                }
            }
        });
    }
});
```

---

# ផ្នែកទី ៤៖ ប្រធានបទសម្រាប់សិក្សាស្រាវជ្រាវបន្ថែម (Further Study Topics)

នៅក្នុងមេរៀននេះ សិស្សនិស្សិតត្រូវបន្តសិក្សាស្រាវជ្រាវបន្ថែមលើសមាសភាគទាក់ទងនឹង Navigation ដូចខាងក្រោម៖
1. **Navigation Drawer**
2. **Bottom Navigation Bar**
3. **TabLayout**

---

# ផ្នែកទី ៥៖ តារាងប្រៀបធៀបសង្ខេបរវាង Menu ទាំង ៣ ប្រភេទ

| លក្ខណៈវិនិច្ឆ័យ | Options Menu | Context Menu | Popup Menu |
| :--- | :--- | :--- | :--- |
| **អាកប្បកិរិយាអ្នកប្រើប្រាស់** | បង្ហាញលើ App Bar ឬពេលចុច overflow button | បង្ហាញពេល **Long-click** លើ View | បង្ហាញពេលចុចលើ View/Button ជាក់លាក់ |
| **ឥទ្ធិពលលើ Content** | ឥទ្ធិពលជាសកលលើ App (Global impact) | ប៉ះពាល់ដោយផ្ទាល់លើ Content ដែលជ្រើសរើស | សកម្មភាពបន្ថែម មិនប៉ះពាល់ Content ដោយផ្ទាល់ |
| **ការភ្ជាប់ទីតាំង (Anchor)** | ផ្នែកខាងលើបង្អស់នៃ Activity (App Bar) | លេចឡើងអណ្តែតលើអេក្រង់ (Floating) | ភ្ជាប់ទៅនឹង View ជាក់លាក់ដែលបានហៅវា |
| **Callback ដំបូងគេ** | `onCreateOptionsMenu()` | `onCreateContextMenu()` | តាមរយៈ `PopupMenu` instance ក្នុង `onClick` |
| **Callback ចាប់ Event** | `onOptionsItemSelected()` | `onContextItemSelected()` | `setOnMenuItemClickListener()` |

---

💡 តើលោកគ្រូ/អ្នកគ្រូ ចង់ឱ្យខ្ញុំរៀបចំប្រឡងតេស្ត (Quiz) ឬបង្កើតស្លាយបទបង្ហាញបន្ថែមលើប្រធានបទ **Chapter 6** នេះដែរឬទេ?
