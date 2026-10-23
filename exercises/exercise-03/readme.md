[Home - AD261](/README.md)

# Exercise 3: Create a Custom ABAP Cloud Application

## Introduction

In this exercise, you will create a simple RAP application as a basis for performance analysis in the subsequent exercises.

### Exercises

- [3.1 - Create a Database Table](#exercise-31-create-a-database-table)
- [3.2 - Generate a RAP Application](#exercise-32-generate-a-rap-application)
- [3.3 - Implement Additional Business Logic](#exercise-33-implement-additional-business-logic-optional)
- [Summary & Next Exercise](#summary-next-exercise)

## Exercise 3.1: Create a Database Table

> Create a new database table **`ZAD261_###_DOC`**.
>
> This database table represents a simplified document (header) table with **ID**, **Name**, and **Status**.

<details>
  <summary> 🔵 Click to expand!</summary>

1. In ADT, go to the **Project Explorer**, right-click the package **`ZAD261_###`**, and select **New** > **Other ABAP Repository Object** > **Database Table** from the context menu.

2. Enter the required information and click **Next >**.
   > Note: **`###`** is your assigned group ID.

   - Name: **`ZAD261_###_DOC`**
   - Description: _**`Document`**_

3. Click **Next >** and then click **Finish**.

4. Define the following database table fields. Note that the field **client** is already added automatically to the initial field list:

```ABAP
  key client            : abap.clnt not null;
  key document_uuid     : sysuuid_x16 not null;
  document_id           : abap.char(10);
  document_name         : abap.sstring(60);
  status                : abap.char(1);
  local_created_by      : abp_creation_user;
  local_created_at      : abp_creation_tstmpl;
  local_last_changed_by : abp_locinst_lastchange_user;
  local_last_changed_at : abp_locinst_lastchange_tstmpl;
  last_changed_at       : abp_lastchange_tstmpl;
```

5. Configure the usage of inverted individual index by adding the header annotation (just to avoid a consistency check warning):

```ABAP
@AbapCatalog.primaryKey.invertedIndividualIndex : true
```

6. Activate the database table by clicking the **Activate** button in the toolbar or by pressing the shortcut **`command+F3`**. The activation includes save and syntax check.

7. The complete database table definition should have the following source code:

```ABAP
@EndUserText.label : 'Document'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
@AbapCatalog.primaryKey.invertedIndividualIndex : true
define table zad261_###_doc {

  key client            : abap.clnt not null;
  key document_uuid     : abap.raw(16) not null;
  document_id           : abap.char(10);
  document_name         : abap.sstring(60);
  status                : abap.char(1);
  local_created_by      : abp_creation_user;
  local_created_at      : abp_creation_tstmpl;
  local_last_changed_by : abp_locinst_lastchange_user;
  local_last_changed_at : abp_locinst_lastchange_tstmpl;
  last_changed_at       : abp_lastchange_tstmpl;

}
```

</details>

## Exercise 3.2: Generate a RAP Application

> Generate all RAP objects for a complete CRUD-enabled (create/retrieve/update/delete) RAP application.

<details>
  <summary> 🔵 Click to expand!</summary>

1. In ADT, go to the **Project Explorer**, right-click the created database table **`ZAD261_###_DOC`**, and select **Generate ABAP Repository Objects...**. Select the generator **OData UI Service**.

2. Click **Next >**. Enter package **`ZAD261_###`** and click **Next**. Do not adjust any of the default values. Click **Next > Next > Finish**.

3. After a few seconds, the RAP application is generated and a popup appears asking whether to open the **service binding** in the ADT editor. Click **Open**. The name of the generated service binding is **`ZUI_AD261_###_DOC_O4`**. The service binding type is **OData V4 - UI**, which is also reflected as a naming convention in the last two characters of the service binding name.

4. The status of the **Local Service Endpoint** is **Unpublished**. Click the **Publish** button to enable system-local testing via the preview of the generated UI service. Publishing takes approximately 1 minute.

5. In section **Service Version Details**, double-click the **Entity Set and Association** **`ZC_AD261_###_DOC`** to open the **Preview** in your default browser. The preview is based on SAP Fiori Elements.

6. Add a new document entry in the app by clicking the button **Create**, enter simple test data into the fields **DocumentID** and **DocumentName**, and press **Save**. The read-only field **DocumentUUID** is filled automatically.

7. Switch to ADT. Open the database table **`ZAD261_###_DOC`** editor and press the shortcut **`F8`** to open the data preview. You will see the entries you created in the app preview.

8. Switch back to the app preview in the browser, or open the preview again as described in step 5. Note that you need to click the **Go** button to display the existing entries in the list.

</details>

## Exercise 3.3: Implement Additional Business Logic (Optional)

> In a subsequent exercise, the number of documents with STATUS = `'E'` (error) will be monitored using a custom metric provider. With the current version of the generated RAP business object, you can simply enter the value `'E'` in the **Status** field in the app preview.
>
> If you are an experienced RAP developer, you may adjust the RAP business object to make the **Status** field read-only in the UI and enable setting STATUS = `'E'` via a new action. This approach more closely reflects typical business logic, where a status value is determined by the outcome of a business process step rather than being entered manually. A business process step is usually initiated by an action.
>
> Since this session focuses on monitoring rather than RAP business object development, you can skip this exercise and set the value of the **Status** field manually in a subsequent exercise.

<details>
  <summary> 🔵 Click to expand!</summary>

1. The field **STATUS** shall be set by an action (to be implemented later) and not changed manually. Therefore, the field **STATUS** shall be read-only in the app.

   The required adjustment to the behavior definition must be made in the generated **Behavior Definition** **`ZR_AD261_###_DOC`**. Open the editor for **Behavior Definition** **`ZR_AD261_###_DOC`** via the Project Explorer and expand your exercise package **`ZAD261_###`** > **Core Data Services** > **Behavior Definitions**. Define the field **`Status`** as read-only by adding the field name to the `field ( readonly )` section. Change the originally generated lines of code

```ABAP
  ...
  field ( readonly )
   LocalCreatedBy,
   LocalCreatedAt,
   LocalLastChangedBy,
   LocalLastChangedAt,
   LastChangedAt;
  ...
```

   to

```ABAP
  ...
  field ( readonly )
   Status,
   LocalCreatedBy,
   LocalCreatedAt,
   LocalLastChangedBy,
   LocalLastChangedAt,
   LastChangedAt;
   ...
```

   Activate the changes by pressing the shortcut **`command+F3`**.

2. Add the declaration for the new action **setErrorStatus** in the root (Z**R**_...) behavior definition **`ZR_AD261_###_DOC`**.

```ABAP
  action setErrorStatus result [1] $self;
```

3. Add the action into the consumption (Z**C**_...) behavior definition **`ZC_AD261_###_DOC`**.

```ABAP
  use action setErrorStatus;
```

4. Make the action visible in the UI by enhancing the existing UI annotations `@UI.identification` and `@UI.lineitem` in the metadata extension **`ZC_AD261_###_DOC`**. Add the two lines starting with `{ type: #FOR_ACTION, ...` to the UI annotations for `DocumentUUID`.

```ABAP
  ...
  @EndUserText.label: 'DocumentUUID'
  @UI.identification: [
    { position: 10, label: 'DocumentUUID' },
    { type: #FOR_ACTION, dataAction: 'setErrorStatus', label: 'Set Error Status', position: 10 }
  ]
  @UI.lineItem: [
    { position: 10, label: 'DocumentUUID' },
    { type: #FOR_ACTION, dataAction: 'setErrorStatus', label: 'Set Error Status', position: 10 }
  ]
  @UI.selectionField: [ {
    position: 10
  } ]
  DocumentUUID;
  ...
```

5. Implement the action by setting STATUS = `'E'` using a local EML statement. You can generate the method definition using a quick fix in the root behavior definition **`ZR_AD261_###_DOC`**. Use the quick fix **Add method for action setErrorStatus** on the left side of the action definition source code line. The quick fix opens the class editor for **`ZBP_R_AD261_###_DOC`**, tab **Local Types**. Add the following source code for method **setErrorStatus**:

```ABAP
  METHOD setErrorStatus.

    MODIFY ENTITIES OF zr_ad261_###_doc IN LOCAL MODE
      ENTITY ZrAd261###Doc
        UPDATE FIELDS ( status )
        WITH VALUE #( FOR key IN keys ( %tky   = key-%tky
                                        status = 'E' ) ). " Error

    READ ENTITIES OF zr_ad261_###_doc IN LOCAL MODE
      ENTITY ZrAd261###Doc
        ALL FIELDS WITH
        CORRESPONDING #( keys )
        RESULT DATA(documents).

    result = VALUE #( FOR document IN documents ( %tky   = document-%tky
                                                  %param = document ) ).

  ENDMETHOD.
```

   Replace all occurrences of the placeholder **`###`** with your group ID. Save and activate the code.

6. Return to the service binding **`ZUI_AD261_###_DOC_O4`** editor. Double-click the **Entity Set and Association** **`ZC_AD261_###_DOC`** to open the **Preview**. The app now shows a new button **Set Error Status**. The button is enabled when at least one entry in the list is selected, and is also available on the detail screen of an entry. Set the error status for a few entries by selecting them and clicking the **Set Error Status** button.

</details>

## Summary & Next Exercise

Now that you have:

- created an ABAP database table and a RAP application based on it,
- optionally implemented an action to set STATUS = `'E'`,

you can continue with the next exercise ► **[Exercise 4: Analyze OData Performance](../exercise-04/readme.md)**.
