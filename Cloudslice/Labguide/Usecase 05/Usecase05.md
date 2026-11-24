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

The Fabric data agent automatically retrieves the relevant data from the lakehouse and generates insights, helping teams quickly understand business performance. This improves productivity and enables faster decision-making across departments.

With this solution, Zava's business users, analysts and management teams can easily explore data, monitor key performance indicators, and get timely insights into sales performance, customer behavior and product demand.

To bring these insights to where people already work, Zava connects the Fabric data agent to a Copilot Studio agent and publishes that agent to Microsoft Teams. Business users can then ask questions in a Teams chat, and the Copilot Studio agent works with the Fabric data agent (agent-to-agent collaboration) to return answers grounded in governed data.

**Objectives**

- Build and configure a **Fabric Data Agent** connected to an e‑commerce semantic model.

- Ingest and model data inside a **Fabric Lakehouse** and expose it through a semantic model.

- Enhance the agent's intelligence by using meta-prompts and agent-level instructions.

- Add the Fabric data agent as a connected agent in a Copilot Studio agent so that both agents work together.

- Publish the Copilot Studio agent and add it to Microsoft Teams for real-time analytics.

- Test the end-to-end flow by querying business insights directly in Teams.

**Prerequisites**

- A paid **F2 or higher Fabric capacity** (or Power BI Premium P1 or higher with Microsoft Fabric enabled).

- Fabric data agent tenant settings enabled, including **cross-geo processing and cross-geo storing for AI**.

- A **Microsoft 365 Copilot license** and a **Copilot Studio user license** for the lab user.

- A Copilot Studio environment with available **Copilot Credits** (prepaid capacity or a pay-as-you-go billing plan). Without credits, the agent fails with the error code EnforcementUsageCredits.

- The same account and tenant for Microsoft Fabric and Microsoft Copilot Studio.

## Exercise 1: Create and configure the Fabric data agent

In this exercise, you build the foundational components in Microsoft Fabric. You create a workspace, set up a lakehouse, ingest sample CSV files, create a semantic model, and configure a Fabric data agent that answers analytical questions. This agent is the core data intelligence layer for the rest of the lab.

### Task 1: Create a Fabric workspace

In this task, you create a Fabric workspace. The workspace holds all the Fabric items for this lab: the lakehouse, the semantic model and the data agent.

1.  Open your browser, enter +++https://app.fabric.microsoft.com/+++ in the address bar, press **Enter**, and sign in with the following credentials.

    | | |
    |----|----|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | Password | +++@lab.CloudPortalCredential(User1).Password+++ |

2.  On the Fabric home page, select the **+ New workspace** tile.

    ![](./media/image1.png)

3.  In the **Create a workspace** pane that appears on the right, enter the following details, and then select **Apply**.

    | **Property** | **Value** |
    |----|----|
    | Name                   | +++Fabric-Copilot-XXXXX+++ (XXXXX = your lab instance ID) |
    | Advanced               | Under License mode, select Fabric                         |
    | Default storage format | Small dataset storage format                              |
    | Template apps          | Check the Develop template apps                           |

    ![](./media/image2.png)

    **Note:** To find your lab instance ID, select **Help** in the lab interface and copy the **ID** value.

    ![](./media/image3.png)

    ![](./media/image4.png)

4.  Wait for the workspace to be created. This takes 2–3 minutes. The new, empty workspace opens.

    ![](./media/image5.png)

### Task 2: Create a lakehouse and ingest sample data

In this task, you create a lakehouse, upload the Zava e-commerce CSV files (orders, customers and products), load them into Delta tables, and create a semantic model over these tables. The semantic model is the data source for the Fabric data agent.

1.  On the workspace page, select **+ New item**.

    ![](./media/image6.png)

2.  In the **Filter by item type** search box, enter +++Lakehouse+++, and then select the **Lakehouse** tile.

    ![](./media/image7.png)

3.  In the **New lakehouse** dialog box, enter +++fabricagent_lakehouse+++ in the **Name** field, leave **Lakehouse schemas** cleared, and then select **Create**. The new lakehouse opens.

    **Note:** Make sure there is no leading space before **fabricagent_lakehouse** in the name.

    ![](./media/image8.png)

    ![](./media/image9.png)

4.  Wait for the **Successfully created SQL analytics endpoint** notification.

    ![](./media/image10.png)

5.  On the lakehouse page, under **Get data in your lakehouse**, select **Upload files**.

    ![](./media/image11.png)

6.  In the **Upload files** pane, select the folder icon under **Files**.

    ![](./media/image12.png)

7.  Browse to **C:\LabFiles** on your VM, select **customers.csv**, **Orders_Data.csv** and **products.csv**, and then select **Open**.

    ![](./media/image13.png)

8.  Select **Upload**. When all three files show as uploaded, close the **Upload files** pane.

    ![](./media/image14.png)

    ![](./media/image15.png)

9.  In the **Explorer** pane, select **Files** to view the uploaded files. If they don't appear, select the **…** next to **Files** and then select **Refresh**.

    ![](./media/image16.png)

10. Hover over **Orders_Data.csv**, select the **…** (ellipsis) next to it, and then select **Load to Tables** \> **New table**.

    ![](./media/image17.png)

11. In the **Load file to new table** dialog box, keep the table name **orders_data** and select **Load**. A notification confirms when the table is created.

    ![](./media/image18.png)

    ![](./media/image19.png)

12. Repeat the same steps for **customers.csv** and **products.csv** to load them into the **customers** and **products** tables.

    ![](./media/image20.png)

    ![](./media/image21.png)

    ![](./media/image22.png)

    ![](./media/image23.png)

    ![](./media/image24.png)

    ![](./media/image25.png)

13. From the lakehouse drop-down menu at the top right of the screen, select **SQL analytics endpoint**.

    ![](./media/image26.png)

14. On the **Home** tab, select **New semantic model**.

    ![](./media/image27.png)

15. In the **New semantic model** dialog box, enter +++E-commerce Order Dataset+++ as the name, select **Select all** to add all tables, and then select **Confirm**.

    ![](./media/image28.png)

16. In the left navigation pane, select the **Fabric-Copilot-XXXX** workspace.

    ![](./media/image29.png)

### Task 3: Create a Fabric data agent

1.  On the **Fabric-Copilot-XXXX** workspace page, select **+ New item**, and then select **Data agent**.

    ![](./media/image30.png)

2.  In the **Create data agent** dialog box, enter +++DataAgent_@lab.LabInstance.Id+++ as the name, and then select **Create**.

    ![](./media/image31.png)

3.  Under **Build your data agent**, select **Add a data source**.

    ![](./media/image32.png)

4.  In the **OneLake catalog** window, select **E-commerce Order Dataset** (type **Semantic model**), and then select **Add**.

    ![](./media/image33.png)

    ![](./media/image34.png)

5.  In the **Explorer** pane, expand **E-commerce Order Dataset** and select all the tables: **customers**, **orders_data** and **products**.

    ![](./media/image35.png)

6.  Test the agent. In the chat box, enter +++Who are the top 10 customers by total purchase amount?+++ and select **Send**. Even without instructions, the agent answers simple questions fairly well.

    ![](./media/image36.png)

    ![](./media/image37.png)

7.  Enter +++Which day has the highest sales?+++, select **Send**, and review the response.

    ![](./media/image38.png)

    ![](./media/image39.png)

### Task 4: Optimize the agent with meta-prompts

1.  On the data agent ribbon, select **Agent instructions**. The **Agent instructions** pane opens.

    ![](./media/image40.png)

2.  In the **Test the agent's responses** pane on the right, paste the following meta-prompt. Replace the placeholders with the values listed after the prompt, and then select **Send**. The agent analyzes its data sources and generates agent-level instructions for itself.

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

    Replace the placeholders with these values:

    | **Placeholder** | **Value** |
    |----|----|
    | {AGENT_OBJECTIVE} | +++E-commerce analytics agent for business intelligence+++ |
    | {USER_PERSONA} | +++Business analysts and sales teams+++ |
    | {RESPONSE_STYLE} | +++Clear summaries with data citations and trend analysis+++ |
    | {CUSTOM_TERMINOLOGY} | Leave empty or add your own domain-specific terms |

3.  Copy the agent-level instructions that the agent generates, paste them into the **Agent instructions** box, and review them. The instructions are saved automatically.

    ![](./media/image41.png)

4.  Test the enhanced agent with more complex questions. Enter each question and select **Send**:

    +++How many orders are placed each day?+++

    ![](./media/image42.png)

    ![](./media/image43.png)

    +++Which products have the lowest stock levels?+++

    ![](./media/image44.png)

    ![](./media/image45.png)

### Task 5: Publish the data agent

1.  In the **Test the agent's responses** pane, paste the following meta-prompt and select **Send**. The agent generates a short description of itself. Copy the description.

    ```text
    Meta-Prompt: Generate Agent Description

    Create a 1-2 sentence description of yourself as a Fabric Data Agent (max 200 chars).
    Analyze your data sources and describe: what data domain you cover and what questions you answer.
    Example: "Fabric Data Agent for retail sales. Answers questions about revenue, products, customers, and orders"
    Output plain text only.
    ```

    ![](./media/image46.png)

2.  On the ribbon, select **Publish**. In the **Publish data agent** dialog box, paste the generated description into **Description of purpose and capabilities**, and then select **Publish**.

    ![](./media/image47.png)

    ![](./media/image48.png)

## Exercise 2: Connect the Fabric data agent to Copilot Studio

In this exercise, you enable Copilot Studio to communicate with the Fabric data agent. You create a Copilot Studio agent, configure its behavior, connect it to the Fabric data agent, and verify that both agents work together to produce richer insights.

### Task 1: Create the Copilot Studio agent

1.  Open a new browser tab and go to +++https://copilotstudio.microsoft.com/+++. On the Copilot Studio home page, select the arrow next to **Explore what else is possible with agents**.

    ![](./media/image49.png)

    ![](./media/image50.png)

2.  Under **Build with standard orchestration**, select **Agent**.

    ![](./media/image51.png)

3.  In the **Name your agent** dialog box, enter +++E-commerce RAG Agent+++, and then select **Create**.

    ![](./media/image52.png)

4.  When the agent is provisioned, select **Edit** in the **Details** section of the **Overview** page.

    ![](./media/image53.png)

5.  Configure the agent with the following settings:

    - **Name**: +++E-commerce RAG Agent+++

    - **Description**: +++An agent connected to a Microsoft Fabric data agent specializing in e-commerce business knowledge and support+++

    - **Model**: select **Claude Sonnet 4.6**

    - **Instructions**: copy and paste the following instructions:

    ```text
    You are the E-commerce RAG Agent for Zava, an online retailer. You help
    business analysts and sales teams answer questions about orders, customers,
    products, prices, stock levels, revenue and payment methods.

    - For any question about Zava's e-commerce data, always call the connected
      Fabric data agent and base your answer only on the data it returns.
    - Never invent numbers. If the data agent returns no data, say so and suggest
      a more specific question.
    - Start with a one-sentence answer, then show the details. Use a table when
      the result has more than three rows.
    - Show currency values with two decimals and state the time period that the
      answer covers.
    - End with one or two key insights and offer a relevant follow-up question.
    - If a question is not about Zava's e-commerce data, politely explain that you
      can only help with e-commerce business questions.
    ```

    ![](./media/image54.png)

6.  Select **Save**. Wait for the **Your request completed successfully** message.

    ![](./media/image55.png)

    ![](./media/image56.png)

7.  Select **Publish** in the top-right corner, and then select **Publish** in the **Publish this agent** dialog box.

    ![](./media/image57.png)

    ![](./media/image58.png)

### Task 2: Add the Fabric data agent as a connected agent

1.  On the agent **Overview** page, scroll to the **Agents** section and select **+ Add agent**.

    ![](./media/image59.png)

2.  In the **Choose how you want to extend your agent** pane, select **Connect to an external agent**, and then select **Microsoft Fabric (preview)**.

    ![](./media/image60.png)

3.  If **Connection** shows **Not connected**, select the drop-down next to it, and then select **Create new connection**.

    ![](./media/image61.png)

4.  In the **Connect to Fabric data agent** dialog box, select **Create**. When prompted, sign in with the same account that you use for this lab.

    ![](./media/image62.png)

    ![](./media/image63.png)

5.  Verify that the connection shows your lab account, and then select **Next**.

    ![](./media/image64.png)

6.  Select your Fabric data agent, **DataAgent_XXXX** (DataAgent_ followed by your lab instance ID), which you created in Exercise 1, Task 3, and then select **Next**.

    ![](./media/image65.png)

7.  Enter +++DataAgent-@lab.LabInstance.Id+++ as the name, verify the connection, and then select **Add and configure**.

    ![](./media/image66.png)

8.  Select **Publish**, and then select **Publish** in the **Publish this agent** dialog box to make the updated agent available.

    ![](./media/image67.png)

    ![](./media/image68.png)

    ![](./media/image69.png)

### Task 3: Test the connected Fabric data agent

1.  In the **Test your agent** pane, enter the following question and select **Send**:

    +++What are the top 10 highest value orders?+++

    ![](./media/image70.png)

2.  When the agent asks to use your credentials to connect to the Fabric data agent, select **Allow**. The agent calls the Fabric data agent and returns the answer, along with the tool call details.

    ![](./media/image71.png)

    ![](./media/image72.png)

    ![](./media/image73.png)

    ![](./media/image74.png)

    ![](./media/image75.png)

    ![](./media/image76.png)

    **Note:** The response can take 5–6 minutes to generate.

3.  Ask the following questions one at a time, and review the responses:

    +++What is the average price per category?+++

    ![](./media/image77.png)

    ![](./media/image78.png)

    +++What percentage of orders use credit card vs PayPal vs debit card?+++

    ![](./media/image79.png)

    ![](./media/image80.png)

    +++What is the revenue by payment method?+++

    ![](./media/image81.png)

    ![](./media/image82.png)

## Exercise 3: Connect the agent to Microsoft Teams

In this exercise, you publish the Copilot Studio agent to Microsoft Teams so that business users can access enterprise data directly from their collaboration app. You then validate the agent by asking several business questions inside Teams.

### Task 1: Add the Microsoft Teams channel

1.  In the **E-commerce RAG Agent**, select the **+3** (more) menu at the top of the page, and then select **Channels**.

    ![](./media/image83.png)

    ![](./media/image84.png)

2.  Select **Teams and Microsoft 365 Copilot**.

    ![](./media/image85.png)

3.  Select **Add channel**.

    ![](./media/image86.png)

4.  Select **See agent in Teams** to open the agent in Microsoft Teams.

    ![](./media/image87.png)

5.  If your browser asks to open the Microsoft Teams app, select **Open**.

    ![](./media/image88.png)

    ![](./media/image89.png)

6.  Select **Sign in**.

    ![](./media/image90.png)

7.  Enter your lab username and select **Next**, enter your password or Temporary Access Pass and select **Sign in**, and then select **Yes** to stay signed in. If the **Enjoy your Teams trial** page appears, select **Start trial**.

    ![](./media/image91.png)

    ![](./media/image92.png)

    ![](./media/image93.png)

    ![](./media/image94.png)

8.  On the **E-commerce RAG Agent** app page, select **Add**.

    ![](./media/image95.png)

9.  After the app is added, select **Open** to launch the **E-commerce RAG Agent** in Microsoft Teams.

    ![](./media/image96.png)

    ![](./media/image97.png)

### Task 2: Test the agent in Microsoft Teams

1.  In the chat box, enter the following question and select **Send**:

    +++What is the revenue trend over time?+++

    ![](./media/image98.png)

2.  When prompted, select **Allow** so that the agent can connect to the Fabric data agent with your credentials.

    ![](./media/image99.png)

    ![](./media/image100.png)

3.  Ask the following questions one at a time, and review the responses:

    +++What are the top 10 highest value orders?+++

    ![](./media/image101.png)

    ![](./media/image102.png)

    +++Which payment method is used the most?+++

    ![](./media/image103.png)

    ![](./media/image104.png)

**Summary**

In this lab, you integrated a Microsoft Fabric data agent with Microsoft Teams through Copilot Studio to give Zava's business users real-time, natural language access to their e-commerce data. You:

- Created a Fabric workspace and lakehouse, loaded the Zava orders, customers and products CSV files into Delta tables, and built the **E-commerce Order Dataset** semantic model.

- Created a Fabric data agent on the semantic model and tested it with business questions.

- Used meta-prompts to generate agent-level instructions and a description for the agent, and then published it.

- Created the **E-commerce RAG Agent** in Copilot Studio and connected the Fabric data agent to it, so that the two agents work together.

- Published the Copilot Studio agent to Microsoft Teams and asked business questions directly in a Teams chat.

With this solution, Zava's business users can get trusted answers about sales trends, products and customer behavior in the collaboration app they use every day, without writing queries or waiting for new reports.
