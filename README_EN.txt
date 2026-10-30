BOLT AND ANCHOR EDITOR FOR ADVANCE STEEL - TEST VERSION (prototype)
(Polish version: README_instrukcja.txt)
==============================================================

WHAT IT IS
An external program for editing the bolt and anchor catalogs in the Advance Steel database, Tekla style:
- bolt sets (washers, nuts) shown from the bolt head to the end of the bolt, new standards and strength classes,
  your own washers and nuts,
- anchors: new lengths, diameters and classes, new anchor standards, a live preview of the anchor shape with
  dimensions, base plate and fastener set.
It works on the AstorBase database (SQL Server LocalDB). Tested with: Advance Steel 2026, national version INT.
Other Advance Steel versions and other national versions have not been checked.

REQUIREMENTS
- Windows and Advance Steel with its database in LocalDB (verified: Advance Steel 2026 INT)
- Internet on the first run (installs Python, the pyodbc module and the ODBC driver if they are missing)
- Administrator approval if Windows asks to install the driver

BEFORE TESTING: BACKUP (mandatory)
1. Close Advance Steel and Management Tools.
2. In a command prompt type:  sqllocaldb stop AdvanceSteel2026
   (check the instance name with  sqllocaldb i )
3. Copy the whole folder  C:\ProgramData\Autodesk\Advance Steel 2026\INT\Steel\Data  to a safe place.
   (INT is the national version, yours may be different.)
The program also makes its own backup (.bak) in the "backups" folder next to it, but a manual copy is safer.

INSTALLATION (recommended)
1. Unpack the WHOLE ZIP archive into any folder (e.g. Downloads).
2. Double-click  Instaluj.bat . The installer installs Python, pyodbc and the ODBC driver if they are missing
   (internet is needed, and the driver may require administrator approval), copies the program to
   %LOCALAPPDATA%\AS_Edytor_Srub and creates a shortcut on the desktop and in the Start menu.
3. Update: unpack the new package and run Instaluj.bat again - settings and backups are kept.
4. Uninstall: Odinstaluj.bat (removes the program and shortcuts; it does not change the Advance Steel database).
If the program does not start from the shortcut, a file named blad.log appears in its folder - send it together
with a screenshot.

RUNNING WITHOUT THE INSTALLER
1. Unpack the package to a normal folder on a disk (do not run it from inside the archive preview).
   The program consists of advance_steel_bolt_editor.py and the as_editor folder - they must be next to each other.
2. Start Advance Steel or Management Tools (the database must be attached).
3. Double-click  uruchom_edytor_srub.bat  (the first run can take a few minutes).
4. In the program window click "Connect" ("Połącz"). The indicator at the top turns green.
   If you use a national version other than INT, click "Detect versions" and select the right one.
5. Language: the "English" / "Polski" button in the top right corner switches the whole application (the choice is
   stored in bolt_editor_settings.json). The window is rebuilt after switching - the database connection and the
   last selected standards are kept.

WINDOW LAYOUT
Three main tabs:
- Anchors - anchor configurator: pick a standard, select a template anchor and use "+ Length", "+ Diameter" or "+ Class".
- Bolt sets - the nut and washer set for a bolt standard (Tekla-like view).
- Tools - less frequent functions: new washer / nut, bolt standards and classes, standard copy, anchor copy,
  deleting my anchors, table browsers.
You can narrow the standard lists (anchors and bolts) by typing text, e.g. "hilti" - only matching items stay;
Enter selects the first one. The anchor configurator also has an anchor type filter (Normal, Headed, Threaded,
Hooked, J). The program remembers the last selected anchor standard, bolt standard and the entry owner
(also after changing the language).

TEST 1: ADDING A LENGTH / DIAMETER / CLASS TO AN ANCHOR ("Anchors" tab)
1. Select an anchor standard (e.g. HILTI HSA) and select the anchor to be used as the template.
2. Click one of the buttons:
   "+ Length" - opens a small window: enter the new lengths and check the dimensions (taken from the selected
     existing length by default). Type, diameter, class and set of the anchor stay unchanged and the new lengths are
     added to this anchor (also a factory one: its existing rows are not changed, the added rows have your owner).
     You can remove added lengths with "Delete selected my lengths" (green rows in the table below the anchor list).
   "+ Diameter" - enter a new diameter and lengths; class, set and dimensions come from the template.
   "+ Class" - enter a new class; diameter, set and dimensions come from the template.
3. Enter lengths as a list or ranges (e.g. 300-1000/100, 1200), click "From-to step..." or paste a column from Excel.
4. The anchor drawing on the right refreshes by itself. The set composition (nuts, washers) is in the left column;
   an item that is missing in the database for this diameter is red with "(missing)". Select it and click
   "Replace..." or tick "Save without the missing items".
5. The "Preview of what will be saved" table shows what will be written (Status NEW / NEW length).
   Click "Save to database".
6. In Advance Steel click "Update defaults" (or restart it) and check the anchor in Management Tools
   (Parameters and Set tabs) and in the model.
Other anchor windows ("Add anchor..." button and the "More" menu): a single anchor in a standard, several diameters
at once (double-click a table cell = edit in the cell, Ctrl+V = paste from Excel: one column is lengths, several
columns are the length and the following dimensions), a new anchor standard. Your own anchors (green) are edited
with the "Edit selected..." button.

TEST 2: BOLT SET ("Bolt sets" tab)
1. Select standard 4014. The drawing and the composition (washers, nut) should appear.
2. Class: click 10.9. Diameter: click one, e.g. Ø 16 (all are selected by default).
3. Set key: enter TEST1 (a different one for each tester, e.g. TEST_JK). Display name: Test NaW.
   To add an ADDITIONAL set based on an existing one: "Copy an existing set...", then enter a NEW key.
4. Change the composition (e.g. add a washer from the list of available items; "Replace..." replaces the selected
   item). Under the preview table you see the summary "To save: ..., skipped: ..." and the reason for skipping
   in the Status column.
5. Click "Save set to database..." and confirm. A backup message and "Saved 1 rows" appear.
6. Check in the program: "Tools" -> "Table browser (bolts)" -> SetOfBolts -> Filter: TEST1.
   There should be a row with OwnerText = MOJA.
7. Check in Advance Steel: close and restart Management Tools / Advance Steel.
   In Management Tools (Bolt -> Set) there should be "Test NaW". In the model insert a bolt
   (standard 4014, class 10.9, diameter 16) and choose this set. Check the 3D model and the BOM.

TEST 3 (optional): TOOLS
- "New washer / nut" - create your own washer based on an existing one (e.g. thickness multiplier 1.5).
  This is how you can supply an item that was missing in a set.
- "Bolt standards and classes" (four sub-tabs):
  1) Class dictionary - type e.g. 5.6 or A4 in Filter. If the class is missing, add it (Key, RunName).
     Note: A4 already exists in the database as class 1.4401 (display name A4).
  2) New bolt standard - choose a template (e.g. 4014 / 8.8), "Load template into table", enter a key e.g. TEST-4014,
     tick a class, enter e.g. 40-60/10 in the Lengths column and click "Create standard".
  3) Add class to standard - standard 4014, source class 8.8, new class 5.6, choose one diameter.
  4) Delete my entries - cleanup after the test (removes only entries with owner MOJA).
- "Copy anchors / add class" - copy anchors to a new standard or add a new class to many anchors at once.

CLEANUP AFTER THE TEST
Bolts: "Tools" -> "Table browser (bolts)": SetOfBolts -> Filter TEST1 -> select rows -> "Delete selected MY rows".
Then the same in the Sets table (entry TEST1). Entries from "Bolt standards and classes": sub-tab 4 (Delete my
entries), and a class from the dictionary can be removed in sub-tab 1 (only when it is no longer used).
Anchors: "Anchors" tab -> "Delete selected" button (a MOJA anchor) or "Tools" -> "Delete my anchors". Note: entries
added on the way (a new source in the Sources table, a new set in Sets, a new class) stay in the dictionaries;
remove the set in the Table browser and the class in the Class dictionary. Factory entries (DSC) are never
deleted by the program - they are protected.

RESTORING THE DATABASE (if something goes wrong)
Close Advance Steel, type  sqllocaldb stop AdvanceSteel2026  and replace the files in the folder
...\Steel\Data with your copy. After Advance Steel starts, the database returns to its state before the tests.

WHAT TO REPORT
- The Advance Steel version and the national version (INT, POL, ...)
- The exact error message (also from the black command prompt window) or a screenshot
- A description: what was clicked, what was expected, what happened

IMPORTANT
- This is a prototype. Do not test it for the first time on a production database without a copy of the Data folder.
- Your own entries have the owner MOJA. An update or repair of Advance Steel may delete them
  (you would then have to create them again).
- Saved changes are visible only after restarting Advance Steel / Management Tools.
