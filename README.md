# Telecom Customer Churn Prediction --- IBM SPSS Modeler

## Project Overview

This project uses **IBM SPSS Modeler** to build a data-mining model that
predicts customer churn for a fictional telecommunications company. The
aim is to identify customers who may cancel their subscriptions so the
company can take steps to retain them.

The workflow uses historical customer data to build a model and then
applies that model to current customer data to identify customers at
risk of churning.

## Objectives

-   Prepare and inspect historical telecom customer data.
-   Select relevant fields for modelling.
-   Build a churn prediction model using a **CHAID decision tree**.
-   Review the model's decision tree and predicted churn categories.
-   Score current customers with the trained model.
-   Identify high-risk customers and export their records for follow-up.

## Tools and Data

-   **Tool:** IBM SPSS Modeler
-   **Modelling technique:** CHAID decision tree
-   **Historical dataset:** `telco x modeling data.xlsx`
-   **Current-customer dataset:** `telco x deployment data.xlsx`

The exercise material references the following Windows data folder:
`C:\Training\OA007`. The datasets are referenced by the project
instructions; they are not included with this README unless supplied
separately.

## Dataset Fields

The historical dataset contains customer details and usage or billing
information. Fields shown in the exercise include:

-   `customer_id`
-   `data_known`
-   `gender`
-   `age`
-   `tariff`
-   `dropped_calls`
-   `handset`
-   `peak_mins`
-   `peak_calls`
-   `offpeak_mins`
-   `offpeak_calls`
-   `bill_opeak`
-   `bill_peak`
-   `offpeak_rate`
-   `bill_opeak`
-   `gadget_A`, `gadget_B`, `gadget_C`, `gadget_D`
-   `gadget_A_revenues`, `gadget_B_revenues`, `gadget_C_revenues`,
    `gadget_D_revenues`
-   `churn`
-   `retention`

Field availability may differ between the historical and deployment
files. In the modelling setup, `churn` is the target field. The exercise
treats `gender`, `age`, `tariff`, `dropped_calls`, and `handset` as
input predictors. The customer identifier and fields not selected as
predictors are not used as model inputs.

## Workflow

### 1. Set up the stream and import data

1.  Open IBM SPSS Modeler and set the working directory.
2.  Add an **Excel source node** to the stream.
3.  Import `telco x modeling data.xlsx`.
4.  Preview the data and inspect the fields, including `churn` and
    `retention`.

### 2. Select records with known outcomes

1.  Add a **Table node** to inspect the dataset.
2.  Use a **Select node** to keep records where `data_known` indicates
    that the customer's data is known.
3.  Run the table again to confirm the selected records.

The exercise reports **31,769 records** after this selection.

### 3. Define field roles

1.  Add a **Type node** after the Select node.
2.  Read the field values and review the measurement types.
3.  Set `gender`, `age`, `tariff`, `dropped_calls`, and `handset` to the
    **Input** role.
4.  Set `churn` to the **Target** role.
5.  Set other fields, including the customer identifier, to **None** for
    modelling.

### 4. Build the churn model

1.  Add a **CHAID node** from the Modelling palette.
2.  Use `churn` as the target field.
3.  Run the node to generate the model nugget.
4.  Open the model's Viewer to inspect the decision tree and its splits.

In the example results, the first split is based on `handset`, followed
by splits involving tariff and other customer groups. The tree can help
highlight segments with higher observed churn.

### 5. Review model predictions

Preview the scored data and review the fields added by the model:

-   `$R-churn`: predicted churn category.
-   `$RC-churn`: confidence associated with the predicted category.

The exercise reports that **14,713 of 31,769 customers (46.3%)** in the
selected historical data had churned. This is a result reported by the
exercise dataset, not a guarantee of performance on new data.

### 6. Score current customers

1.  Import `telco x deployment data.xlsx` in a second stream branch.
2.  Use the same field preparation approach as needed.
3.  Apply the CHAID model nugget built from the historical data.
4.  Review the predicted churn category and confidence for each current
    customer.

### 7. Identify and export customers at risk

1.  Add a Select node after scoring.
2.  Configure it to keep customers predicted as `Churned` with
    `$RC-churn > 0.9`.
3.  Rename the prediction and confidence fields to clearer labels, such
    as `predicted value` and `probability to churn`.
4.  Use a Filter node to remove `customer_id` from the exported fields
    if required by the exercise and retain the prediction fields.
5.  Add a **Flat File** export node and name the output
    `customers at risk.dat`.
6.  Run the export to create a file that can be used by other
    departments for customer-retention follow-up.

The threshold of 0.9 is the threshold specified in the exercise
instructions; it is not presented as a universally optimal business
threshold.

## Results

The exercise demonstrates a complete churn-prediction workflow:

-   Historical records are selected and prepared for modelling.
-   A CHAID decision tree is trained with `churn` as its target.
-   The model assigns predicted churn categories and confidence values.
-   The model is applied to current customer records.
-   Customers predicted to churn with confidence greater than 0.9 are
    exported to `customers at risk.dat`.

The exercise material states that the completed stream is available in a
`Solutions` folder under the training directory.

## How to Reproduce

1.  Install and open IBM SPSS Modeler.
2.  Obtain the two Excel datasets named above.
3.  Set the working directory to the folder containing the datasets.
4.  Follow the workflow sections in order, connecting the nodes as
    described.
5.  Run the stream and check the Table outputs at each stage.
6.  Export the selected at-risk customer records to
    `customers at risk.dat`.

Exact menu labels may vary by IBM SPSS Modeler version.

## Important Notes

-   This README documents the workflow described in the supplied project
    instructions.
-   The project uses fictional telecommunications customer data, as
    described in the exercise.
-   A predicted churn category and its confidence are model outputs, not
    proof that a particular customer will churn.
-   Before using such predictions in a real business setting, evaluate
    model quality on suitable held-out data and review whether the
    selected threshold is appropriate.

## Project Summary

**Input:** Historical and current telecommunications customer datasets\
**Model:** CHAID decision tree\
**Target:** Customer churn (`churn`)\
**Output:** A scored dataset and an export of customers identified as at
risk (`customers at risk.dat`)
