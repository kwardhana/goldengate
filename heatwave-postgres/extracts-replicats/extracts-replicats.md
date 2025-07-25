# Create OCI PostgreSQL Replicats

## Introduction

In the previous lab, you created Extracts and Distribution Paths to perform an initial load and capture change data. This lab walks you through the steps to create Replicats on the target PostgreSQL deployment to deliver the data sent from the source MySQL deployment.

Estimated time: 15 minutes

### About Replicats

A Replicat is a process that delivers data to a target database.

### Objectives

In this lab, you will:
* Add a checkpoint table
* Create a Replicat for Initial Load
* Create a Replicat for Change Data Capture

### Prerequisites

* This lab assumes that you completed all preceding labs succesfully
* Your deployment is in the Active state.

## Task 1: Add a checkpoint table

1.  In the target PostgreSQL OCI GoldenGate deployment console (**OCIPG_instance**), click **Administration Server**.

2.  Open the navigation menu, and then click **DB Connections**.

3.  For the TargetADW database, click **Connect to database TargetPG**.

    ![Connect to TargetPG](./images/01-03-dbconnect.png " ")

4.  For Checkpoint, click **Add Checkpoint** (plus icon).

    ![Click Add Checkpoint](./images/01-04-add-checkpoint.png " ")

5.  For Checkpoint table, enter `GGS.CHECKTABLE`, and then click **Submit**. The checkpoint table is added to the list.

    ![Checkpoint Table panel](./images/01-05-checkpoint-panel.png " ")

6.  In the navigation menu, click **Overview** to return to the Administration Service Overview page.

## Task 2: Create a Replicat for Initial Load

1.  On the Administration Service Overview page, click **Add Replicat** (plus icon).

    ![Click Add Replicat on Administration Service Overview page](./images/02-01-add-replicat.png " ")

2. The Add Replicat panel consists of four pages. On the Replicat Information page, complete the following fields, and then click **Next**:

    * For **Replicat Type**, select **Classic Replicat**.
    * For **Process Name**, enter a name for this process. For example, `RIL`.

  ![Source Options page](./images/02-02-replicat-type.png " ")

3. On the Replicat Options page, complete the following fields, and then click **Next**:

    * For Replicat Trail **Name**, enter the name of the trail from the previous lab (`I1`). 
    * For **Domain**, select the domain for the OCI Postgre SQL connection.
    * For **Alias**, select the alias for the OCI Postgre SQL connection.
    * For **Checkpoint Table**, select the checkpoint table created in Task 1.

    ![Replicat options](./images/02-03-replicat-options.png " ")

4. On the Managed Options page, leave the fields as they are, and then click **Next**.

    ![Managed Options page](./images/02-04-man-opts.png " ")

5.  On the Parameter Files page, replace `MAP *.*, TARGET *.*;` with the following mapping, and then click **Create and Run**:

    ```
    <copy>MAP SRC_OCIGGLL.*, TARGET SRCMIRROR_OCIGGLL.*;</copy>
    ```

    ![Replicat Parameter File](./images/02-05-params.png " ")

    You return to the Overview page where you can review the Replicat details.

6.  Select the RIL Replicat and view its details.

7.  Click **Statistics**. Review the number of inserts, then refresh the page.
    * If the number of Inserts doesn't change, then all the records from the Initial Load have been loaded and you can stop the Replicat (RIL).
    * If the number of Inserts continues to increase, then keep refreshing the page until the Initial Load records are all loaded before continuing.

      ![Replicat statistics](./images/02-07-rep-stats.png " ")

Click **Administration Service** to return to the Overview page.

## Task 3: Create a Replicat for Change Data Capture

1.  On the Administration Service Overview page, click **Add Replicat** (plus icon).

    ![Click Add Replicat on Administration Service Overview page](./images/03-01-add-replicat.png " ")

2. The Add Replicat panel consists of four pages. On the Replicat Information page, complete the following fields, and then click **Next**:

    * For **Replicat Type**, select **Classic Replicat**.
    * For **Process Name**, enter a name for this process. For example, `RCDC`.

3. On the Replicat Options page, complete the following fields, and then click **Next**:

    * For Replicat Trail **Name**, enter the name of the trail from the previous lab (`C1`). 
    * For **Domain**, select the domain for the Autonomous Database connection.
    * For **Alias**, select the alias for the Autonomous Database connection.
    * For **Checkpoint Table**, select the checkpoint table created in Task 1.

4.  On the Parameter Files page, add the following mapping:

    ```
    <copy>MAP SRC_OCIGGLL.*, TARGET SRCMIRROR_OCIGGLL.*;</copy>
    ```

5.  Click **Create**. Don't run the Replicat just yet.

6.  On the Administration Service Overview page, select the Replicat for Initial Load (RIL) and view its Details.

7.  Click **Statistics**. Review the number of inserts, then refresh the page.
    * If the number of Inserts doesn't change, then all the records from the Initial Load have been loaded and you can stop the Replicat (RIL)
    * If the number of Inserts continues to increase, then keep refreshing the page until the Initial Load records are all loaded before continuing.

8.  Return to the Administration Service Overview page and select **Start** from the Change Data Capture Replicat (RCDC) **Action** menu.

9.  After the Change Data Capture Replicat (RCDC) starts successfully, select it, and then click **Statistics**.

You may now **proceed to the next lab.**

## Learn more

* [Add a Replicat for Oracle Database](https://docs.oracle.com/en/cloud/paas/goldengate-service/cress/index.html)

## Acknowledgements
- **Author** - Katherine Wardhana, User Assistance Developer
- **Contributors** -  Shrinidhi Kulkarni, GoldenGate Product Manager
- **Last Updated by** - Katherine Wardhana, June 2025