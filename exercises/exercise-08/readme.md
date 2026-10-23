[Home - AD261](/README.md)

# Exercise 8: Fix the COMPUTE_INT_ZERODIVIDE Runtime Error (Optional)

## Introduction

In this optional exercise, you will fix the intentional programming error with the help of the ADT MCP Server and an AI CLI of your choice.

### Exercises

- [8.1: Enable ADT MCP Server](#exercise-81-enable-adt-mcp-server)
- [8.2: Fix the Runtime Error with AI](#exercise-82-fix-the-runtime-error-with-ai)
- [8.3: Test the Fixed Version of the Console Application](#exercise-83-test-the-fixed-version-of-the-console-application)
- [Summary](#summary)

## Exercise 8.1: Enable ADT MCP Server

> Set up ADT to work with the AI CLI or AI chat of your choice using the ADT MCP Server. The ADT MCP Server is part of the ADT shipment — both ADT for Eclipse and ADT for VS Code. The MCP tools required to analyze a short dump are available starting with ADT for Eclipse version **3.62**, delivered in October 2026. This [blog post](https://community.sap.com/t5/technology-blog-posts-by-sap/abap-development-tools-mcp-server-major-update-available/ba-p/14496064) provides more information about the updated ADT MCP Server tooling.

<details>
  <summary> 🔵 Click to expand!</summary>

1. In ADT for Eclipse, go to **Window** > **Preferences** (or in case of macOS: **Eclipse** > **Settings**), select **ABAP Development** > **MCP Server**, and enable the option **Enable ADT MCP Server**.

2. Make the ADT MCP Server available to the AI CLI or AI chat of your choice. Copy the **port** and **token** from the above-mentioned ADT settings to your connection configuration.

3. In the AI CLI or AI chat of your choice, verify the visibility of the ADT MCP Server by either inspecting the MCP tools list using the respective built-in feature or by entering a prompt such as `list all MCP tools`. The MCP tool `abap_feed_query` is a prerequisite for reading short dumps and is therefore also required to fix the source code.

4. To allow AI to fix the root cause of the runtime error, enable write access in the ADT MCP Server settings. Navigate to the same ADT settings window as described in step 1. In the **Edit File Tool** section, enable **Enable Edit File tool for ABAP objects**. In the **Access Restrictions** section, enable **Enable access restriction** and enter your package name **`ZAD261_###`** in the **Packages** field.

**Important note**: As stated in this [blog post](https://community.sap.com/t5/technology-blog-posts-by-sap/abap-development-tools-mcp-server-major-update-available/ba-p/14496064) in section _Safety policy_:
> The ADT MCP server follows a _safety by default_ policy. This means everything is switched off by default. It is up to the user to switch on what is needed and manage the associated risks accordingly. It follows the same _human in the loop_ principle which applies to AI in general.

</details>

## Exercise 8.2: Fix the Runtime Error with AI

> Trigger the fix via a prompt. An alternative would be an agent-based approach, which is not shown here.

<details>
  <summary> 🔵 Click to expand!</summary>

1. Enter the following prompt:

```
Fix the dump in system T01 in class zcl_ad261_###_dump
```

2. Follow the instructions in the CLI or chat window and decide on the access permissions to read and write data and the ABAP class.

3. Review the proposed changes before activation. The proposed change will most likely add a `TRY...CATCH cx_sy_zerodivide` block around the division. The original version was:

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

   The fixed version should look similar to this:

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
      TRY.
          out->write( 100 / i ).
        CATCH cx_sy_zerodivide.
          out->write( |Division by zero at i = { i }| ).
      ENDTRY.
      i -= 1.
    ENDDO.

  ENDMETHOD.

ENDCLASS.
```

4. Save and activate the code.

</details>

## Exercise 8.3: Test the Fixed Version of the Console Application

> Execute the console application again. It should now produce console output instead of raising an ABAP runtime error.

<details>
  <summary> 🔵 Click to expand!</summary>

1. Execute the global class **`ZCL_AD261_###_DUMP`** by opening its context menu in the Project Explorer and clicking **Run As** > **ABAP Application (Console)**, or by pressing the shortcut **`F9`**.

2. The expected console output for the fixed version is:

```
20
25
33
50
100
Division by zero at i = 0
-100
-50
-33
-25
```

</details>

## Summary

Now that you have fixed the original programming error, you have seen how to analyze and fix the root cause of short dumps with AI assistance.
