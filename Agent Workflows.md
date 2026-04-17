Workflow Overview
What is a Workflow?
A workflow is an automation system that orchestrates multiple AI agents to perform complex tasks step by step.
Workflow Types
```
# Microsoft Foundry - Workflows provide templates by type.

Single Agent → Sequential Workflow → Group Chat → Human-in-loop
(Simple)                                                (Complex)
```
Type	Description	Use Cases
Sequential	Sequential execution	Data pipelines, document processing
Group Chat	Conversation between agents	Collaborative problem solving, decision making
Human-in-loop	Human intervention	Approval processes, validation
Workflow Components
    Workflow
     ├─ Template (Sequential / Human / Group)
     ├─ Nodes
     │   ├─ Agent Node
     │   ├─ Logic Node (If/Else, For Each, Go To)
     │   ├─ Data Transformation Node (data processing and variable management)
     │   └─ User Interaction Node (send messages, ask questions)
     ├─ Agents
     ├─ Execution Context
     └─ Run & Save

---
1. Sequential Workflow
Agents and nodes execute serially in a predetermined order.
The output of the previous step is passed as input to the next step.
Suitable for pipelines and step-by-step processing.
Key Scenarios
Document input -> Summarization -> Classification -> Response generation
Ticket processing pipeline
RAG preprocessing -> Response generation -> Post-processing

Switch to the New Foundry UI
In the Microsoft Foundry Portal (https://ai.azure.com), enable the new Foundry toggle in the upper right corner, then select the project created in the previous lab.
<img width="1992" height="1125" alt="image" src="https://github.com/user-attachments/assets/3c1b81d4-dc31-4f74-9daa-02c3edbc7721" />
Click the Build menu in the upper right to see the Foundry Build menus on the left.
You are now ready to create agents and workflows.
Create Required Agents
First, click Create Agent to create the agents to be used in the workflow.
TravelPlannerAgent
Agent name: TravelPlannerAgent
Model: gpt-5.2 (You can select a different GPT model deployed in the previous lab.)
Instructions:
    ```
    You are a travel planning expert.
    
    Role:
    1. Analyze the user's travel requirements
    2. Recommend key attractions, restaurants, and accommodations at the destination
    3. Create a detailed day-by-day travel itinerary
    4. Provide estimated costs and a packing list
    
    Output format:
    - Destination overview
    - Daily itinerary (morning/lunch/evening activities)
    - Recommended accommodations
    - Estimated costs
    - Packing list
    
    Information to pass to the next agent: Complete travel plan
    ```
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/388da360-ecb6-4d21-bdda-4337c83d18c1" />
LocalAgent
Agent name: LocalAgent
Model: gpt-5.2 (You can select a different GPT model deployed in the previous lab.)
Instructions:
    ```
    You are a local information expert.
    
    Role:
    1. Receive the travel plan from the previous agent
    2. Use Web search to find the latest local information
    3. Add real-time information:
       - Current weather and climate
       - Local festivals and events
       - Transportation info (routes, fares, travel time)
       - Business hours and reservation info
       - Local culture and tips
    
    Output format:
    - Original itinerary + enhanced with local information
    - Detailed transportation information
    - List of places requiring reservations
    - Local tips
    
    Information to pass to the next agent: Travel plan with local information added
    ```
Add the Web Search tool.
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/a75e7b9c-3802-46e2-a9bc-ca5fc03df2e3" />
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/40ad45c1-3197-408e-b983-7b7a42d19f40" />
TravelSummaryAgent
Agent name: TravelSummaryAgent
Model: gpt-5.2 (You can select a different GPT model deployed in the previous lab.)
Instructions:
    ```
    You are a travel plan organizer expert.
    
    Role:
    1. Consolidate information from the previous agents
    2. Organize into a final actionable plan
    3. Generate checklists
    
    Output format:
    📋 Travel Summary
    - Destination: 
    - Duration:
    - Budget:
    
    📅 Itinerary Summary (at-a-glance schedule)
    
    ✅ Pre-departure Checklist
    - [ ] Item 1
    - [ ] Item 2
    
    🎒 Packing Checklist
    
    📞 Emergency Contacts and Useful Information
    
    Final output: Printable travel guide
    ```
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/ac6bafcf-ae85-499a-9fc0-5dfdc9794312" />
    <img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/b2a296b8-aec6-48ed-a354-297995399e95" />

Create Sequential Workflow
Create a New Workflow
Workflows > Create > Sequential: Create a workflow using the Sequential Workflow template.
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/0e0c193d-6c1f-48dc-ba07-6f30c34cd80a" />
Add Agents
Add the agents created earlier in order.
For each step: Select agent -> Change Task ID -> Done.
Step 1
Task ID: TravelPlanner
Select agent: TravelPlannerAgent
Step 2
Task ID: LocalSearch
Select agent: LocalAgent
Step 3
Task ID: TravelSummary
Select agent: TravelSummaryAgent
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/85716da4-ea7f-4ace-9079-941834d8e3cd" />

Save Workflow
Click the Save button.
Workflow name: Sequential-TravelPlan
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/8eb61936-2fbc-44bf-b1fc-5884a73099cd" />

Test the Workflow
Preview Mode
Click the Preview button.
Request a travel plan.
Test Question
```
   Create a 2-night, 3-day travel plan for Jeju Island
   ```
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/6eb27906-7536-40d1-a14b-00b0c1e9fb28" />

Observe the Execution Process
Check the output at each step:
Step 1 (TravelPlannerAgent): Generate basic travel itinerary
Step 2 (LocalAgent): Add local information (weather, transportation, events)
Step 3 (TravelSummaryAgent): Final summary and checklists
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/e8f3a92c-847a-4bd0-83ae-d325e3507d90" />
   <img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/cc208cb0-94df-4806-9e91-ff22c3285f70" />

Check Traces
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/e8036a3c-6406-4159-8aa5-77d31f524607" />
Execution time for each agent
Data passed between agents
Final output generation process
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/dcf47012-3dd6-4e8a-bd9d-e74dfda4e8d9" />
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/638526e4-32f3-4fac-a3bd-e6c30a476ab6" />
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/8e4e6d56-b40f-4208-a11e-54669909f272" />
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/7470f051-ef1e-4a14-bff5-a9e74c001d2c" />

Deploy and Invoke the Workflow
Publish
Click the Publish button.
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/93f7d7a4-0df3-41e0-98e0-c7f0cdd327d4" />
Confirm the workflow name and version, then publish.
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/8a29c46c-fe6e-48bd-8d54-cb69b9ebe16f" />

Invoke with Python SDK
Download invokeworkflow.py and open the file in Visual Studio Code.
In the code, update the `PROJECT_ENDPOINT`, `WORKFLOW_NAME`, and `WORKFLOW_VERSION` values to match your environment.
You can find the PROJECT_ENDPOINT on the Foundry Portal home screen.
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/bbbb6ba1-f49d-4f61-9040-f93fb67cb9d0" />
Run python invokeworkflow.py from the terminal.
```
   python invokeworkflow.py
   ```
> 💡 **Lab Tip**: The code below is for reference only. For the actual lab, open the `invokeWorkflow.py` file in the root path of this repository and update the `PROJECT_ENDPOINT`, `WORKFLOW_NAME`, and `WORKFLOW_VERSION` values to match your environment before running.
`invokeWorkflow.py` file example:
```python
   # Microsoft Foundry Workflow Invocation using Foundry SDK
   # Before running: pip install --pre azure-ai-projects>=2.0.0b1
   from azure.identity import DefaultAzureCredential
   from azure.ai.projects import AIProjectClient
   from azure.ai.projects.models import ResponseStreamEventType
   
   # Project configuration
   PROJECT_ENDPOINT = "https://<foundry-resource-name>.services.ai.azure.com/api/projects/proj-default"
   WORKFLOW_NAME = "Sequential-Workflow"
   WORKFLOW_VERSION = "1"  # Update to the published version
   
   # Create AI Project client
   project_client = AIProjectClient(
       endpoint=PROJECT_ENDPOINT,
       credential=DefaultAzureCredential(),
   )
   
   with project_client:
       workflow = {
           "name": WORKFLOW_NAME,
           "version": WORKFLOW_VERSION,
       }
       
       # Get OpenAI client from project
       openai_client = project_client.get_openai_client()
   
       # Create a conversation
       conversation = openai_client.conversations.create()
       print(f"Created conversation (id: {conversation.id})")
   
       # Call the workflow with streaming
       print(f"\nCalling workflow: {WORKFLOW_NAME}...\n")
       stream = openai_client.responses.create(
           conversation=conversation.id,
           extra_body={"agent": {"name": workflow["name"], "type": "agent_reference"}},
           input="Create a 2-night, 3-day travel itinerary for Jeju Island",
           stream=True,
           metadata={"x-ms-debug-mode-enabled": "1"},
       )
   
       # Process streaming events
       for event in stream:
           if event.type == ResponseStreamEventType.RESPONSE_OUTPUT_TEXT_DONE:
               print("\t", event.text)
           elif event.type == ResponseStreamEventType.RESPONSE_OUTPUT_ITEM_ADDED and event.item.type == "workflow_action":
               print(f"\n{'='*60}")
               print(f"Actor - '{event.item.action_id}':")
               print(f"{'='*60}")
           elif event.type == ResponseStreamEventType.RESPONSE_OUTPUT_ITEM_DONE and event.item.type == "workflow_action":
               print(f"\n✓ Workflow Item '{event.item.action_id}' is '{event.item.status}'")
               print(f"  (previous item was: '{event.item.previous_action_id}')")
           elif event.type == ResponseStreamEventType.RESPONSE_OUTPUT_TEXT_DELTA:
               print(event.delta, end="", flush=True)
   
       # Clean up
       print("\n\n✅ Workflow completed!")
       openai_client.conversations.delete(conversation_id=conversation.id)
       print("Conversation deleted")
   ```
Run
```bash
   pip install --pre azure-ai-projects>=2.0.0b1
   python invokeWorkflow.py
   ```
✅ Verification Checklist
Verify that all agents execute in order
Verify that each agent's output is passed to the next agent
Verify that the final output is generated correctly
<img width="1772" height="1125" alt="image" src="https://github.com/user-attachments/assets/525f0d62-41a0-4969-87eb-73e621e58954" />
<img width="1772" height="1125" alt="image" src="https://github.com/user-attachments/assets/1c17a884-6313-4c8a-bc51-7ccab1de86f7" />
<img width="1772" height="1125" alt="image" src="https://github.com/user-attachments/assets/31e551d4-f8a4-46f8-9940-7f5379f14118" />

---
2. Group Chat Workflow
A workflow where multiple agents collaborate through conversation to solve problems.
Multiple agents pass control based on context/pool
Dynamic routing rather than a fixed pipeline
Key Scenarios
Expert agent handoff
Escalation / Fallback structure
Specialized agent collaboration (Student <-> Teacher)
Create Required Agents
First, click Create Agent to create the agents to be used in the workflow.
StudentAgent
Agent name: StudentAgent
Model: gpt-5.2 (You can select a different GPT model deployed in the previous lab.)
Instructions:
    ```
    You are an agent that answers questions. When a question comes in, always provide an answer.
    
    Role:
    1. Understand the user's question and generate a response
    2. Provide a basic answer on the first attempt
    3. Improve the answer based on TeacherAgent's feedback
    4. Revise the answer until all requirements are met
    
    Considerations when answering:
    - Schedule (dates, times)
    - Cost (budget, prices)
    - Preferences (tastes, style)
    - Constraints (limitations, conditions)
    
    If improvement is needed, incorporate TeacherAgent's feedback to enhance the answer.
    ```
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/ebb83190-7c10-404d-9211-5e5d7ef6ddc7" />
2. TeacherAgent
Agent name: TeacherAgent
Model: gpt-5.2 (You can select a different GPT model deployed in the previous lab.)
Instructions:
    ```
    You are an agent that evaluates answers. If the answer considers various conditions such as schedule, cost, and preferences, respond with [COMPLETE]. Otherwise, do not mark COMPLETE and request revisions.
    
    Evaluation criteria:
    1. Schedule: Are specific dates, times, and durations included?
    2. Cost: Is budget, pricing, and cost information included?
    3. Preferences: Were the user's preferences or style considered?
    4. Practicality: Is the plan actually executable?
    5. Completeness: Is all necessary information included?
    
    Response format:
    When evaluation is complete: "[COMPLETE] All criteria have been met."
    When improvement is needed: "Please address the following: [specific feedback]"
    
    Important: Use [COMPLETE] only when all criteria are satisfied.
    ```
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/f85c2419-657e-44e1-ac7e-d8e097305a2b" />
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/6767906f-87ca-4292-b1a0-9417a3f4995f" />

Create Group Chat Workflow
Create a New Workflow
Workflows > Create > Group Chat: Create a workflow using the Group Chat Workflow template.

Add Agents
Add the agents created earlier in order.
For each step: Select agent -> Change Task ID -> Done.
Agent call 1
Task ID: student_agent
Select agent: StudentAgent
Agent call 2
Task ID: teacher_agent
Select agent: TeacherAgent
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/2262226e-a20c-403c-abf6-a7426885ae35" />

Configure Conversation Flow
```
   User → StudentAgent → TeacherAgent → StudentAgent → ...
   ```
StudentAgent provides the initial answer
TeacherAgent evaluates and gives feedback
Loop continues until [COMPLETE] appears
Conversation ends after 4 turns
If/Else condition check: End if the message contains 'COMPLETE'
If
```
         !IsBlank(Find("[COMPLETE]", Upper(Last(Local.LatestMessage).Text)))
         ```
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/9657a96b-bab8-4fd2-83ea-ed6df9a2d380" />
Else if: Send a message if the conversation has reached 4 turns
```
         Local.TurnCount >= 4
         ```
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/0ed7484f-65d6-4041-9bd2-f6c100b12989" />
Otherwise: Repeat Student and Teacher conversation

Save Workflow
Click the Save button.
Workflow name: GroupChat
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/c471c531-612a-46ea-b514-220442385336" />

Test the Workflow
Preview Mode
Click the Preview button.
Request a travel plan.
Test Question
```
   Create a 2-night, 3-day travel plan for Seattle
   ```
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/1b76f095-a656-4a89-b24b-a4a29eddfeaa" />
   <img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/b7f8c103-36b0-4c17-9639-3f55884101bf" />
Observe the Execution Process
Check the output at each step:
StudentAgent: Generate answer matching the query
TeacherAgent: Feedback on StudentAgent's answer
Conversation end condition: Check if 'Complete' appears in the conversation or if the StudentAgent-TeacherAgent dialogue has repeated 4 times
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/e8f3a92c-847a-4bd0-83ae-d325e3507d90" />
   <img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/cc208cb0-94df-4806-9e91-ff22c3285f70" />

Check Traces
Click the Debug button.
Check the detailed execution for each workflow step:
Execution time for each agent
Data passed between agents
Final output generation process
<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/11d4dbc2-9d09-4aba-87c8-c69ac2eeb750" />

💡 Group Chat Tips
Role division: Assign clear roles to each agent
Termination conditions: Clear exit conditions to prevent infinite loops
Maximum turns: Set a maximum turn count as a safety measure
Feedback specificity: More specific feedback from TeacherAgent yields better improvements
✅ Verification Checklist
Verify that conversations between agents flow naturally
Verify that TeacherAgent's evaluation criteria are appropriate
Verify that the workflow terminates on the [COMPLETE] condition
---
3. Human-in-loop Workflow
A pattern that pauses the workflow at points requiring human approval or input.
Waits for user input during workflow execution, then resumes
Used for approvals, confirmations, and additional information gathering
Key Scenarios
Approval process for AI results
Contract, report, and policy document review
Escalation to a human when confidence is low

Human-in-loop Workflow Scenario
Based on the Human-in-Loop workflow template and the TravelPlannerAgent created in the Sequential Workflow section above:
[User query] -> TravelPlannerAgent -> [User follow-up query] -> TravelPlannerAgent -> [User ends conversation]
Create Human-in-Loop Workflow
Create a New Workflow
Workflows -> Create
Select the Human-in-Loop Workflow template.
<img width="1688" height="1125" alt="image" src="https://github.com/user-attachments/assets/59e378a5-c245-4f56-906f-add0c135dfa1" />
Add Agent
Click the + icon after the variable setup to add an agent call, select TravelPlannerAgent, and change the Task ID to 'TravelPlanner':
<img width="1688" height="1125" alt="image" src="https://github.com/user-attachments/assets/bb905a78-52ba-4a86-b8f3-dceb810d13dd" />
<img width="1688" height="1125" alt="image" src="https://github.com/user-attachments/assets/5d8ac955-5446-4122-b882-f0fce9b2efd9" />
Modify Human Intervention Condition
In the Ask a Question node, change the question text and click Done.
```
   Are you satisfied with the travel plan? If so, please enter YES.
   ```
<img width="1688" height="1125" alt="image" src="https://github.com/user-attachments/assets/1159cf4f-9307-46a6-b516-bbf39ce5ddf0" />
Enter the condition in the 'If' node of the If/Else condition, then click Done.
```
   Local.ConfirmedInput <> "YES"
   ```
<img width="1688" height="1125" alt="image" src="https://github.com/user-attachments/assets/7ca9a26f-dc18-4bd0-b1fa-d6b504da0f66" />
In the Go To node, select 'Agent call: TravelPlanner' under Select action, then click Done.
Modify Exit Message
Delete the Send Message node after the If branch.
<img width="1688" height="1125" alt="image" src="https://github.com/user-attachments/assets/ac283ef2-334e-4ec3-a089-d8521b1d42d0" />
Modify the message in the Send Message node after the Else branch.
```
   Thank you for using Travel Agency.
   ```
Save Workflow
Click Save and save as 'Human-in-Loop'.

Test the Workflow
Preview Mode
Click the Preview button.
User Question
```
   Create a 2-night, 3-day travel plan for Chicago.
   ```
Check TravelPlannerAgent's response.
Request additional information relevant to the travel season.
```
   Also tell me what to prepare considering January weather
   ```
Review the travel plan tailored to Chicago's January weather.
<img width="1688" height="1125" alt="image" src="https://github.com/user-attachments/assets/240919a6-c121-4a45-854d-e52c049c567e" />
End the conversation.
```
   YES
   ```
<img width="1688" height="1125" alt="image" src="https://github.com/user-attachments/assets/f7e41dd4-78a7-46a5-95cc-c6e96db6ee9d" />

Observe the Execution Process
Check the output at each step:
Step 1: Generate basic travel plan
Step 2: Response to follow-up queries
Step 3: Conversation ends based on user's termination intent

💡 Human-in-loop Best Practices
```
✅ Recommended:
- Clearly mark approval points
- Set timeouts to prevent indefinite waiting
- Provide context to users (previous conversation summary)
- Offer simple approval options (Yes/No/Modify)

❌ Avoid:
- Too many approval points
- Unclear approval questions
- Long timeouts (degrades user experience)
- Structures that don't allow reversal after approval
```
✅ Verification Checklist
Verify that the workflow pauses correctly at approval points
Verify that it branches appropriately based on approval/rejection
Verify that timeouts work correctly
---
📚 Additional Resources
Microsoft Foundry Workflows Overview
Microsoft Agent Framework Workflows Orchestrations Patterns
