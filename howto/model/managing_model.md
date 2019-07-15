# Managing Models

In **Model**, click **Edit** for the model you select. Go to **Feature Definition** , use the **Export Model** , **Import Model** buttons to manage your existing model, such as editing existing model elements (attributes, measuring points, events, services) or creating new model elements.

## About This Task

After creating a model, you may have to change its elements or add new elements as required. This task helps you manage existing models by using EnOS-defined JSON files.

## Before You Start

You need to have proper authority to view and manage models. Contact your OU administrator for access. For more information, see [Policy, Role, and Access](/docs/iam/en/latest/access_policy).

## Procedure

To edit your existing model in a JSON file, export the model first, then edit the downloaded JSON file and import it into the existing model. You can also import the JSON file into a new model to fast create elements.

### Exporting 

1. Go to **Model**, click **Edit** for the model you select.

2. Go to **Feature Definition**, click **Export Model**.

3. Click **Export Model** to download the model file in JSON.

  The downloaded JSON file is named model_Modelname_Timestamp.json, where `Modelname` is inherited from the **Model Name** field from EnOS, and `Timestamp` is the time you download this JSON file in *hhmmssSS*, SS indicating millisecond.

  Edit the JSON file to modify existing model elements or add new elements.

### Importing

1. On **Feature Definition**, click ** Import Model**.

2. In the pop-up, click **Upload**. Select the local JSON file.

3. Click **Import** if your uploaded file passes format verification.
 
 You may have to correct format issues if there are format issues. Refer to the error messages prompted after you have uploaded the JSON file.

## Results

The model you selected has been edited as the changes you made to the JSON file.

