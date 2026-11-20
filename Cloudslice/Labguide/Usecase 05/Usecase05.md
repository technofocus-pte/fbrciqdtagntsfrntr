---
lab:
  title: 'Exercise 1: Creating and Configuring the Fabric Data Agent'
  description: This exercise focuses on enabling Copilot Studio to communicate with the Fabric Data Agent. You will create a Copilot agent, configure its behavior, link it to the Fabric Agent, and ensure that both agents collaborate to produce richer insights. This establishes multi‑agent communication across platforms.
  duration: 5 minutes
  level: 300
  islab: true
---

# Use Case 05: Integrate a Fabric data agent with Copilot Studio and Microsoft Teams

**Introduction**

In today’s competitive digital marketplace, e-commerce companies generate large volumes of data from customer transactions, product catalogs, website interactions, and payment systems. Extracting meaningful insights from this data is essential for improving customer experience, optimizing operations, and increasing revenue. However, managing and analyzing large datasets from multiple sources can be complex without a unified analytics platform.

**Zava**, a rapidly growing e-commerce company, sells various consumer products through its online platform and processes thousands of orders every day. The company collects data from different systems including order management, customer profiles, product inventory, and payment transactions. As Zava’s business grows, the organization faces challenges in analyzing this data efficiently and providing real-time insights to business teams.

To address these challenges, Zava implements a modern analytics solution using **Microsoft Fabric**. Fabric provides a unified platform that integrates data engineering, data storage, data transformation, and business intelligence capabilities within a single environment. Zava stores its raw and processed e-commerce data in the Fabric Lakehouse, enabling scalable data management and analytics.

In addition, Zava leverages **Microsoft Fabric Data Agents** to enhance data accessibility and insights generation. Fabric Agents allow business users and analysts to interact with enterprise data using natural language queries. Instead of manually searching through reports or writing complex queries, users can simply ask questions such as:

- “What are the top selling products this month?”

- “Which region generated the highest sales revenue?”

- “What is the customer order trend over the last quarter?”

The Fabric Agent automatically retrieves relevant data from the Lakehouse and generates insights, helping teams quickly understand business performance. This intelligent interaction improves productivity and enables faster decision-making across departments.

With this solution, Zava’s business users, analysts, and management teams can easily explore data, monitor key performance indicators, and gain real-time insights into sales performance, customer behavior, and product demand. By combining advanced analytics with AI-powered Fabric Agents, Zava builds a scalable and intelligent e-commerce analytics platform that supports data-driven growth and operational excellence.

**Objectives**

- Build and configure a **Fabric Data Agent** connected to an e‑commerce semantic model.

- Ingest and model data inside a **Fabric Lakehouse** and expose it through a semantic model.

- Enhance the agent’s intelligence using **meta‑prompts** and agent‑level instructions.

- Add the Fabric Data Agent as a tool (Fabric IQ Data MCP) in a Copilot Studio agent so that both agents work together.

- Publish the Copilot agent and integrate it into **Microsoft Teams** for real‑time analytics.

- Test the end‑to‑end flow by querying business insights directly in Teams.

**Prerequisites**

- A paid **F2 or higher Fabric capacity** (or Power BI Premium P1 or higher with Microsoft Fabric enabled).

- Fabric data agent tenant settings enabled, including **cross-geo processing and cross-geo storing for AI**.

- A **Microsoft 365 Copilot license** and a **Copilot Studio user license** for the lab user.

- A Copilot Studio environment with available **Copilot Credits** (prepaid capacity or a pay-as-you-go billing plan). Without credits, the agent fails with the error code EnforcementUsageCredits.

- The same account and tenant for Microsoft Fabric and Microsoft Copilot Studio.

## Exercise 1: Creating and Configuring the Fabric Data Agent

In this exercise, you will establish the foundational components in Microsoft Fabric. You will create a workspace, set up a lakehouse, ingest sample CSV datasets, generate a semantic model, and configure a Fabric Data Agent capable of answering analytical questions. This provides the core data intelligence layer used throughout the rest of the lab.

### Task 1: Create a Fabric workspace

In this task, you create a Fabric workspace. The workspace contains all the items needed for this lakehouse tutorial, which includes lakehouse, dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and reports.

1.  Open your browser, navigate to the address bar, and type or paste the following URL: +++https://app.fabric.microsoft.com/+++, press **Enter**, and sign in with the following credentials:

    | | |
    |----|----|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | Password | +++@lab.CloudPortalCredential(User1).Password+++ |

2.  On the Fabric home page, select the **+ New workspace** tile.

    ![](./media/image1.png)

3.  In the **Create a workspace** pane that appears on the right side, enter the following details, and click on the **Apply** button.

    | **Property** | **Value** |
    |----|----|
    | Name          | +++Fabric-Copilot-XXXXX+++ (XXXXX can be your lab instance ID) |
    | Advanced      | Under License mode, select Fabric                        |
    | Default storage format | Small dataset storage format              |
    | Template apps | Check the Develop template apps                          |

    ![](./media/image2.png)

    **Note:** To find your lab instance ID, select **Help** and copy the instance ID.

    ![](./media/image3.png)

    ![](./media/image4.png)

4.  Wait for the deployment to complete. It takes 2-3 minutes to complete.

    ![](./media/image5.png)

### Task 2: Create a lakehouse and Ingest sample data

In this task, you set up a lakehouse and ingest the Zava e-commerce sample data (customers, orders, and products CSV files). This establishes your raw dataset foundation inside Fabric, enabling you to start transformations and queries later.

1.  Create a new lakehouse by clicking on the **+New item** button in the navigation bar.

    ![](./media/image6.png)

2.  On the **Filter by item type** search box, enter +++Lakehouse+++ and select the lakehouse item.

    ![](./media/image7.png)

3.  On the **New lakehouse** dialog box, enter +++fabricagent_lakehouse+++ in the **Name** field, click on the **Create** button and open the new lakehouse.

    **Note:** Ensure that there is no space before **fabricagent_lakehouse**.

    ![](./media/image8.png)

    ![](./media/image9.png)

4.  Wait for the notification stating **Successfully created SQL endpoint**.

    ![](./media/image10.png)

5.  In the **lakehouse** page, navigate to **Get data in your lakehouse** section, and click on **Upload files**.

    ![](./media/image11.png)

6.  On the **Upload files** tab, select the folder icon under **Files**.

    ![](./media/image12.png)

7.  Browse to **C:\LabFiles** on your VM, then select **customers.csv**, **Orders_Data.csv** and **products.csv**, and then click on **Open** button.

    ![](./media/image13.png)

8.  Click the **Upload** button, and then close the **Upload files** pane.

    ![](./media/image14.png)

    ![](./media/image15.png)

9.  Click and select refresh on the **Files**. The file appears.

    ![](./media/image16.png)

10. On the **Lakehouse** page, in the **Explorer** pane, select **Files**. Hover over the **Orders_Data.csv** file, and then select the horizontal ellipsis **(…)** beside **Orders_Data.csv**. Navigate and click on **Load Table**, then select **New table**.

    ![](./media/image17.png)

11. In the **Load file to new table** dialog box, click on the **Load** button.

    ![](./media/image18.png)

    ![](./media/image19.png)

12. Repeat the same process for **customers.csv** and **products.csv** to convert them into tables.

    ![](./media/image20.png)

    ![](./media/image21.png)

    ![](./media/image22.png)

    ![](./media/image23.png)

    ![](./media/image24.png)

    ![](./media/image25.png)

13. Select **SQL analytics endpoint** from the **Lakehouse** drop-down menu at the top right of the screen.

    ![](./media/image26.png)

14. From the lakehouse **Home** tab, select **New semantic model** and select the tables that you want to add to the semantic model.

    ![](./media/image27.png)

15. In the **New semantic model** dialog enter +++E-commerce Order Dataset+++ and then select **all** tables from the list and select **Confirm** to create the new model.

    ![](./media/image28.png)

16. From the left menu, select **Fabric-Copilot-XXXX** workspace icon and then select workspace name.

    ![](./media/image29.png)

### Task 3: Create a Fabric data agent

1.  In the **Fabric-Copilot-XXXX** workspace page, navigate and click on **+New item** button, then select **Data agent**

    ![](./media/image30.png)

2.  Enter +++DataAgent_@lab.LabInstance.Id+++ as the name, and then click **Create**.

    ![](./media/image31.png)

3.  Select **Add data source** to configure a new data source.

    ![](./media/image32.png)

4.  Select **E-commerce Order Dataset** (Type: Semantic Model) from the results.

    ![](./media/image33.png)

    ![](./media/image34.png)

5.  In the **Explorer** pane, select **all tables** of the semantic model. With all tables selected, the data agent answers questions fairly well.

    ![](./media/image35.png)

6.  For example, enter the following question in the test pane:

    +++Who are the top 10 customers by total purchase amount?+++

    ![](./media/image36.png)

    ![](./media/image37.png)

7.  Enter another sample question to verify the response:

    +++Which day has the highest sales?+++

    ![](./media/image38.png)

    ![](./media/image39.png)

### Task 4: Optimizing with Meta-Prompts

1.  In the **Setup** section, locate the **Agent instructions** field. (Alternatively, you can also locate the **Agent instructions** in the navigation bar on the top.)

    ![](./media/image40.png)

2.  Generate agent-level instructions using this meta-prompt in the **test pane** (on the right where it says **Test the agent's responses**):

    ```text
    Meta-Prompt: Generate Agent-Level Instructions:

    Analyze your available data sources and create agent-level instructions for yourself (max 15000 chars).

    Objective: {AGENT_OBJECTIVE}
    Users: {USER_PERSONA}

    Examine your data sources: list all sources, types, and primary use. Analyze domain, time coverage, and main themes.

    Generate instructions with:
    ## Objective
    ## Data Sources (list with priority)
    ## Key Terminology (infer from columns/measures)
    ## Response Guidelines
    Style: {RESPONSE_STYLE}
    ## Handling Common Topics (3-5 based on available data)
    Custom terms: {CUSTOM_TERMINOLOGY}
    ```

    Replace the variables in the prompt with the values below, **or** paste these values in the test pane after the prompt:

    | **Variable** | **Value** |
    |----|----|
    | {AGENT_OBJECTIVE} | +++E-commerce analytics agent for business intelligence+++ |
    | {USER_PERSONA} | +++Business analysts and sales teams+++ |
    | {RESPONSE_STYLE} | +++Clear summaries with data citations and trend analysis+++ |
    | {CUSTOM_TERMINOLOGY} | Leave empty or add your domain-specific terms |

    ![](./media/image41.png)

3.  Test the enhanced agent with more complex queries:

    +++How many orders are placed each day?+++

    ![](./media/image42.png)

    ![](./media/image43.png)

    +++Which products have the lowest stock levels?+++

    ![](./media/image44.png)

    ![](./media/image45.png)

### Task 5: Publishing the Agent

1.  Generate the agent description using this meta-prompt inside the test pane of your Fabric Agent:

    ```text
    Meta-Prompt: Generate Agent Description

    Create a 1-2 sentence description of yourself as a Fabric Data Agent (max 200 chars).
    Analyze your data sources and describe: what data domain you cover and what questions you answer.
    Example: "Fabric Data Agent for retail sales. Answers questions about revenue, products, customers, and orders"
    Output plain text only.
    ```

    ![](./media/image46.png)

2.  Click **Publish** and paste the generated description in the purpose and capabilities field.

    ![](./media/image47.png)

    ![](./media/image48.png)

## Exercise 2: Connecting Fabric Agent to Copilot Studio

In this exercise, you will create a Copilot Studio agent, configure its instructions, and add the published Fabric Data Agent to it as a tool by using the **Fabric IQ Data MCP** server. The Copilot Studio agent decides when to call the Fabric Data Agent, which runs in Fabric and returns answers grounded in your OneLake data.

### Task 1: Creating the Copilot Studio Agent

1.  Open a new browser tab and navigate to +++https://copilotstudio.microsoft.com/+++.

    ![](./media/image49.png)

2.  In the left navigation, select **Agents**.

    ![](./media/image50.png)

3.  On the **Agents** page, select the **arrow next to New agent**, and then select **Agent** under **Optimize your business processes**.

    **Note:** Select the arrow, not **New agent** directly. Only this agent type supports adding a Fabric Data Agent as a tool.

    ![](./media/image51.png)

    ![](./media/image52.png)

4.  Configure your agent with these settings:

    - **Name**: +++E-commerce RAG Agent+++

    ![](./media/image53.png)

    - **Instructions**: Copy the instructions from the code block below and paste them into the **Instructions** box.

    ```text
    You are an e-commerce analytics assistant for Zava.
    Use the Fabric Data Agent tool to answer questions about orders, customers, products, categories, prices, payment methods and revenue.
    Always call the Fabric Data Agent tool for questions that need data. Do not make up numbers.
    Present results as clear summaries. Use tables for rankings and comparisons.
    If a question is unclear, ask a short follow-up question before answering.
    ```

    ![](./media/image54.png)

5.  Select **Save**, and then select **Publish** in the top-right corner. In the **Publish** pane, select **Publish agent**, and then select **Done**.

    ![](./media/image55.png)

    ![](./media/image56.png)

    ![](./media/image57.png)

### Task 2: Adding the Fabric Data Agent as a tool in Copilot Studio

1.  On the **Build** tab, in the pane on the right, go to **Tools** and select the plus sign (**+**).

    ![](./media/image58.png)

2.  In the **Add a tool** dialog, enter +++Fabric+++ in the search box, and then select **Fabric IQ Data MCP**.

    ![](./media/image59.png)

3.  Select **Add**. If you are prompted for a connection, create a new connection and sign in with the same lab credentials that you used for Microsoft Fabric.

    ![](./media/image60.png)

4.  In the **Select an agent to connect** dialog, select **DataAgent_@lab.LabInstance.Id**, and then select **Connect**.

    **Note:** Only published Fabric data agents appear in this list. If you don't see your data agent, confirm that you completed Exercise 1, Task 5.

    ![](./media/image61.png)

5.  The data agent now appears under **Tools**. Select it to open its details.

    ![](./media/image62.png)

6.  In the **Fabric IQ details** dialog, enter the following **Description**, select **Maker** as the **Authentication mode**, and then select **Save**.

    +++Fabric Data Agent for Zava e-commerce data. Answers questions about orders, order values, customers, products, categories, prices, payment methods and revenue.+++

    **Note:** The orchestrator uses this description to decide when to call the data agent. With **Maker** authentication, the agent uses your credentials to query the data. With **User** authentication, each user needs access to the data agent and its data sources.

    ![](./media/image63.png)

7.  Select **Save**, and then select **Publish** \> **Publish agent** to apply the changes.

    ![](./media/image64.png)

    ![](./media/image65.png)

### Task 3: Testing the Fabric Data Agent tool

1.  Select the **Preview** tab, and test the Fabric Data Agent tool with progressive queries:

    **Note:** Testing uses Copilot Credits. If you see the message **You need credits to continue** with the error code **EnforcementUsageCredits**, the environment has no Copilot Credits. Contact your instructor or administrator to add credits.

    +++What are the top 10 highest value orders?+++

    ![](./media/image66.png)

    ![](./media/image67.png)

2.  Click **Allow** to grant the required permissions

    ![](./media/image68.png)

    ![](./media/image69.png)

    ![](./media/image70.png)

    ![](./media/image71.png)

    **Note:** The response generation process may take **5–6 minutes** to complete.

3.  Enter the next question:

    +++What is the average price per category?+++

    ![](./media/image72.png)

    ![](./media/image73.png)

4.  Enter the next question:

    +++What percentage of orders use credit card vs PayPal vs debit card?+++

    ![](./media/image74.png)

    ![](./media/image75.png)

5.  Enter the next question:

    +++What is the revenue by payment method?+++

    ![](./media/image76.png)

    ![](./media/image77.png)

## Exercise 3: Connect the Fabric Data Agent to Teams

In this exercise, you will publish the Copilot agent to Teams, enabling business users to access enterprise data directly from within their collaboration app. You will validate the agent’s functionality by running several BI queries and observing real‑time responses inside Teams.

### Task 1: Add Copilot Capabilities

1.  In the **E-commerce RAG Agent**, click the **+ (Add)** icon and select **Channels** to configure the agent channel settings.

    ![](./media/image78.png)

    ![](./media/image79.png)

2.  Select **Teams and Microsoft 365 Copilot**

    ![](./media/image80.png)

3.  Click **Add channel**.

    ![](./media/image81.png)

4.  Select **See agent in Teams** to open and test the agent in **Microsoft Teams**.

    ![](./media/image82.png)

5.  Click **Open Microsoft Teams**

    ![](./media/image83.png)

    ![](./media/image84.png)

6.  Click **Sign in**.

    ![](./media/image85.png)

7.  Enter your provided credentials to sign in and continue

    ![](./media/image86.png)

    ![](./media/image87.png)

    ![](./media/image88.png)

    ![](./media/image89.png)

8.  Click **Add**

    ![](./media/image90.png)

9.  After the app is added successfully, click the **Open** button to launch the E‑commerce RAG Agent in Microsoft Teams

    ![](./media/image91.png)

    ![](./media/image92.png)

### Task 2: Testing the connected Fabric Data Agent

1.  Test the Fabric Data Agent connection with progressive queries:

    +++What is the revenue trend over time?+++

    ![](./media/image93.png)

2.  Click **Allow** to grant the required permissions

    ![](./media/image94.png)

    ![](./media/image95.png)

3.  Enter the next question:

    +++What are the top 10 highest value orders?+++

    ![](./media/image96.png)

    ![](./media/image97.png)

4.  Enter the next question:

    +++Which payment method is used the most?+++

    ![](./media/image98.png)

    ![](./media/image99.png)

**Summary**

This use case focuses on how an e‑commerce organization can integrate **Microsoft Fabric Data Agents** with **Microsoft Teams** through **Copilot Studio** to deliver real‑time insights, natural‑language analytics, and multi‑agent collaboration. By combining a unified analytics platform (Microsoft Fabric) with conversational AI (Copilot Studio and Teams), business users can seamlessly access sales trends, product insights, and customer behavior without writing queries. The solution demonstrates how AI agents can retrieve data from Fabric Lakehouse, enrich responses using instructions, and work alongside other agents to streamline business intelligence workflows.
