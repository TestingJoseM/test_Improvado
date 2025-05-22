# test_Improvado - José Luis Muñoz
Technical Exam for Improvado.io

# Links:
https://docs.google.com/spreadsheets/d/1Vuq3_wMFkuloT4o2RzXgQ42dipi5sKMlAZxML5K13B0/edit?usp=sharing 

## High-Level Flow
<img width="1214" alt="high-level workflow" src="https://github.com/user-attachments/assets/6fd8ab29-2379-4762-a55d-577ebe0f77b2" />

## Tools Used
1. Make.com (Tools: Multiple Variables, String Composing)
   > Why Make.com? Easier to debug, to filter data, flexible HTTP Requests, cost-effective (as it offers 1,000 ops per month, and doesn't need to be self hosted for this particular workflow).
3. HTTP Request (API Call to Google Gemini)
4. Gemini-1.5-flash

## Overview
This Make.com automation streamlines the onboarding process by automatically assigning a workspace email using the new hire’s full name and setting their primary email group based on their role. All data is pulled from a Google Sheet.

## Modules
1. Trigger: G. Sheets | Watch New Rows Added
2. Multiple Variables: Split the Full Name in 2 variables (firstName and lastName)
3. String Composition: To create the work email with the right format (firstName.lastName@improvado.io). Making sure it's all lower-case for formatting purposes.
4. HTTP Request: A POST API call is made to Google Gemini to make the LLM (Gemini 1.5 Flash) determine the Google Primary Group based on the role. Making sure it always ends with @improvado.io.
   > Why Gemini 1.5 Flash? It's low latency (faster responses for real-time apps), made for lightweight tasks and cost-effective.
   > Parameters: Temperture -> 0.7, maxOutputTokens -> 150
5. String Composition: The message for the new hire was crafted based on it's Full Name, Role and the work email previously generated.
6. Sring Composition: The message for the IT was created with all the details of the new hire incluiding the assigned Google Primary Group assigned by the LLM.
7. Google Sheets: As it's bad practice to end a workflow on a tool, I included an Update Row module to update the Sheet with the fields "Work Email" and "Primary Group" for each new hire.
   

## Suggested Improvements 
1. Add Search for existing emails and auto-increment duplicates
   > To prevent email collisions across hires.
2. Instead of relying on Watch Rows as trigger (there's a 15min delay on Free Plan), I'd use Google Aps Script + Webhook to fire instantly when a new row is added.
   > Real-time onboarding and no polling limit.
3. Use Error Handling to prevent infinite reruns.
4. Send a personalized welcome Slack message with the onboarding steps.
5. Add the user to the suggested Google Group using Google Admin SDK.


