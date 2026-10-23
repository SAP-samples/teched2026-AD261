[Home - AD261](/README.md)

# Exercise 1: Create a Console Application

## Introduction

In this exercise, you will create a simple console application to generate an ABAP **short dump** as a result of an [ABAP runtime error](https://help.sap.com/docs/abap-cloud/abap-development-tools-user-guide/abap-runtime-errors-and-short-dumps?version=sap_btp). ABAP runtime errors occur due to an unhandled or unhandleable exception, a resource or system problem, or an error in the code. Such ABAP runtime errors must be monitored alongside other critical events and situations. The console application deliberately triggers a runtime error by performing a division by zero without proper exception handling — a straightforward programming error.

### Exercises

- [1.1 - Create Package](#exercise-11-create-package)
- [1.2 - Create Console Application](#exercise-12-create-console-application)
- [1.3 - Execute Console Application](#exercise-13-execute-console-application)
- [1.4 - Notice Runtime Error in ADT](#exercise-14-notice-runtime-error-in-adt)
- [Summary & Next Exercise](#summary-next-exercise)

> [!TIP]
>
> - Always replace all occurrences of the placeholder **`###`** in the provided code snippets with your personal suffix.
> - Use the ADT function _**Find and Replace All**_ (**Ctrl+F**) to quickly replace text in the source code.
> - Use the ADT function _**Quick Fix**_ (**Ctrl+1**), aka _Quick Assist_, on an erroneous element to get help with resolving the issue.
> - Use the **Show ABAP element info** view (**F2**) to inspect an element in ADT editors.
> - Use the **ABAP Formatter** function (**Ctrl+F1**) to format your source code.
> - [Useful Keyboard Shortcuts for ABAP Development](https://help.sap.com/docs/ABAP_PLATFORM_NEW/c238d694b825421f940829321ffa326a/4ec299d16e391014adc9fffe4e204223.html?version=latest) (ADT shortcuts)

## Exercise 1.1: Create Package

> Create your exercise package **`ZAD261_###`**.
>
> This ABAP package will contain all the artifacts you will be creating in the different exercises of this hands-on session.

<details>
  <summary> 🔵 Click to expand!</summary>

1. In ADT, go to the **Project Explorer**, right-click the package **`ZLOCAL`**, and select **New** > **ABAP Package** from the context menu.

2. Enter the required information and click **Next >**.

   > Note: **`###`** is your assigned group ID.

   - Name: **`ZAD261_###`**
   - Description: _**`AD261`**_
   - Select the box `☑️` **Add to favorite packages**
   - Superpackage: **`ZLOCAL`**

3. Verify that the Package Type is _Development_ and the Software Component is _ZLOCAL_. Click **Next >** and then click **Finish**.

</details>

## Exercise 1.2: Create Console Application

> Create your global class **`ZCL_AD261_###_DUMP`** as console application and activate the class.
>
> This class contains a programming error (division by zero) in method **`MAIN`**.

<details>
  <summary> 🔵 Click to expand!</summary>

1. In ADT, go to the **Project Explorer**, right-click the package **`ZAD261_###`**, and select **New** > **ABAP Class** from the context menu.

2. Enter the required information and click **Next >**.

   > Note: **`###`** is your assigned group ID.

   - Name: **`ZCL_AD261_###_DUMP`**
   - Description: _**`Generate short dump`**_
   - Interfaces: Add... **`IF_OO_ADT_CLASSRUN`**

3. Click **Next >** and then click **Finish**.

4. Implement the method **`if_oo_adt_classrun~main`** with the following code:

```ABAP
    DATA(i) = 5.
    DO 10 TIMES.
      out->write( 100 / i ).
      i -= 1.
    ENDDO.
```

5. Optionally, you can format the code (pretty print) with the **ABAP Formatter**, see tip above.

6. Activate the class by clicking the **Activate** button in the toolbar or by pressing the shortcut **`command+F3`**. The activation includes save and syntax check.

7. The entire class should have the source code:

```ABAP
CLASS zcl_ad261_###_dump DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_ad261_###_dump IMPLEMENTATION.

  METHOD if_oo_adt_classrun~main.

    DATA(i) = 5.
    DO 10 TIMES.
      out->write( 100 / i ).
      i -= 1.
    ENDDO.

  ENDMETHOD.

ENDCLASS.
```

8. Optionally, you can run the ATC check for the class by opening the context menu of the class in the Project Explorer and clicking **Run As** > **ABAP Test Cockpit**, or by pressing the shortcut **`shift+command+F2`**. The ADT view **ATC Problems** should display the result **Findings: None**.

</details>

## Exercise 1.3: Execute Console Application

> Execute your global class **`ZCL_AD261_###_DUMP`**.
>
> The execution results in an ABAP runtime error.

<details>
  <summary> 🔵 Click to expand!</summary>

1. Execute the global class (more precisely, the **`if_oo_adt_classrun~main`** method) by opening the context menu of the class in the Project Explorer and clicking **Run As** > **ABAP Application (Console)**, or by pressing the shortcut **`F9`**.

</details>

## Exercise 1.4: Notice Runtime Error in ADT

> Observe what happens during the execution of the console application.

<details>
  <summary> 🔵 Click to expand!</summary>

1. Repeat the execution of the global class as described in exercise [1.3 - Execute Console Application](#exercise-13-execute-console-application) a few times.

2. The execution brings the ADT view **Console** into focus. However, there is no output.

3. Look for the small notification popup **ABAP Runtime Error** in the lower-right corner of ADT, showing the category **COMPUTE_INT_ZERODIVIDE**. The popup disappears automatically after a few seconds.

</details>

## Summary & Next Exercise

Now that you have:

- created an ABAP package,
- generated a short dump with a console application that contains a division-by-zero programming error,
- observed the short dump indicator in ADT,

you can continue with the next exercise ► **[Exercise 2: Analyze ABAP Runtime Errors](../exercise-02/readme.md)**.
