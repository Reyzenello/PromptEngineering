# PromptEngineering 

Here are the main techniques that I have gathered reading papers

Here are my notes which I have gathered meanwhile I have done courses and these are the prompts which I have used mainly:

Zero-shot Prompting
Role Prompting
Providing New Information in the Prompt
Chain-of-thought Prompting


Zero-Shot prompting
Here is an example of zero-shot prompting:
You are prompting the model to see if it can infer the task from the structure of your prompt.
In zero-shot prompting, you only provide the structure to the model, but without any examples of the completed task.

prompt = """
Message: Hi Amit, thanks for the thoughtful birthday card!
Sentiment: ?
"""
response = llama(prompt)
print(response)


Few-shot Prompting
Here is an example of few-shot prompting:
In few-shot prompting, you not only provide the structure to the model, but also two or more examples.
You are prompting the model to see if it can infer the task from the structure, as well as the examples in your prompt.
prompt = """
Message: Hi Dad, you're 20 minutes late to my piano recital!
Sentiment: Negative

Message: Can't wait to order pizza for dinner tonight
Sentiment: Positive

Message: Hi Amit, thanks for the thoughtful birthday card!
Sentiment: ?
"""
response = llama(prompt)
print(response)

Role Prompting
Roles give context to LLMs what type of answers are desired.
Llama 2 often gives more consistent responses when provided with a role.
First, try standard prompt and see the response.

prompt = """
How can I answer this question from my friend:
What is the meaning of life?
"""
response = llama(prompt)
print(response)

Now, try it by giving the model a "role", and within the role, a "tone" using which it should respond with:

role = """
Your role is a life coach \
who gives advice to people about living a good life.\
You attempt to provide unbiased advice.
You respond in the tone of an English pirate.
"""

prompt = f"""
{role}
How can I answer this question from my friend:
What is the meaning of life?
"""
response = llama(prompt)
print(response)

Providing New Information in the Prompt
A model's knowledge of the world ends at the moment of its training - so it won't know about more recent events.
Llama 2 was released for research and commercial use on July 18, 2023, and its training ended some time before that date.
Ask the model about an event, in this case, FIFA Women's World Cup 2023, which started on July 20, 2023, and see how the model responses.

prompt = """
Who won the 2023 Women's World Cup?
"""
response = llama(prompt)
print(response)

As you can see, the model still thinks that the tournament is yet to be played, even though you are now in 2024!
Another thing to note is, July 18, 2023 was the date the model was released to public, and it was trained even before that, so it only has information upto that point. The response says, "the final match is scheduled to take place in July 2023", but the final match was played on August 20, 2023.
You can provide the model with information about recent events, in this case text from Wikipedia about the 2023 Women's World Cup.
context = """
The 2023 FIFA Women's World Cup (Māori: Ipu Wahine o te Ao FIFA i 2023)[1] was the ninth edition of the FIFA Women's World Cup, the quadrennial international women's football championship contested by women's national teams and organised by FIFA. The tournament, which took place from 20 July to 20 August 2023, was jointly hosted by Australia and New Zealand.[2][3][4] It was the first FIFA Women's World Cup with more than one host nation, as well as the first World Cup to be held across multiple confederations, as Australia is in the Asian confederation, while New Zealand is in the Oceanian confederation. It was also the first Women's World Cup to be held in the Southern Hemisphere.[5]
This tournament was the first to feature an expanded format of 32 teams from the previous 24, replicating the format used for the men's World Cup from 1998 to 2022.[2] The opening match was won by co-host New Zealand, beating Norway at Eden Park in Auckland on 20 July 2023 and achieving their first Women's World Cup victory.[6]
Spain were crowned champions after defeating reigning European champions England 1–0 in the final. It was the first time a European nation had won the Women's World Cup since 2007 and Spain's first title, although their victory was marred by the Rubiales affair.[7][8][9] Spain became the second nation to win both the women's and men's World Cup since Germany in the 2003 edition.[10] In addition, they became the first nation to concurrently hold the FIFA women's U-17, U-20, and senior World Cups.[11] Sweden would claim their fourth bronze medal at the Women's World Cup while co-host Australia achieved their best placing yet, finishing fourth.[12] Japanese player Hinata Miyazawa won the Golden Boot scoring five goals throughout the tournament. Spanish player Aitana Bonmatí was voted the tournament's best player, winning the Golden Ball, whilst Bonmatí's teammate Salma Paralluelo was awarded the Young Player Award. England goalkeeper Mary Earps won the Golden Glove, awarded to the best-performing goalkeeper of the tournament.
Of the eight teams making their first appearance, Morocco were the only one to advance to the round of 16 (where they lost to France; coincidentally, the result of this fixture was similar to the men's World Cup in Qatar, where France defeated Morocco in the semi-final). The United States were the two-time defending champions,[13] but were eliminated in the round of 16 by Sweden, the first time the team had not made the semi-finals at the tournament, and the first time the defending champions failed to progress to the quarter-finals.[14]
Australia's team, nicknamed the Matildas, performed better than expected, and the event saw many Australians unite to support them.[15][16][17] The Matildas, who beat France to make the semi-finals for the first time, saw record numbers of fans watching their games, their 3–1 loss to England becoming the most watched television broadcast in Australian history, with an average viewership of 7.13 million and a peak viewership of 11.15 million viewers.[18]
It was the most attended edition of the competition ever held.
"""


Now test it out:

prompt = f"""
Given the following context, who won the 2023 Women's World cup?
context: {context}
"""
response = llama(prompt)
print(response)
Chain-of-thought Prompting
LLMs can perform better at reasoning and logic problems if you ask them to break the problem down into smaller steps. This is known as chain-of-thought prompting.

prompt = """
15 of us want to go to a restaurant.
Two of them have cars
Each car can seat 5 people.
Two of us have motorcycles.
Each motorcycle can fit 2 people.

Can we all get to the restaurant by car or motorcycle?
"""
response = llama(prompt)
print(response)

Response:
Yes, all 15 people can get to the restaurant by car or motorcycle.

Here's how:

Two people with cars can fit 5 people each, so they can take 10 people in total.
Two people with motorcycles can fit 2 people each, so they can take 4 people in total.

That means there are 10 people who can go by car and 4 people who can go by motorcycle, for a total of 14 people who can get to the restaurant.

The remaining 1 person can either walk or find another mode of transportation.

Provide the model with additional instructions.


prompt = """
15 of us want to go to a restaurant.
Two of them have cars
Each car can seat 5 people.
Two of us have motorcycles.
Each motorcycle can fit 2 people.

Can we all get to the restaurant by car or motorcycle?

Think step by step.
Explain each intermediate step.
Only when you are done with all your steps,
provide the answer based on your intermediate steps.
"""
response = llama(prompt)
print(response)

The order of instructions matters!
Ask the model to "answer first" and "explain later" to see how the output changes.

prompt = """
15 of us want to go to a restaurant.
Two of them have cars
Each car can seat 5 people.
Two of us have motorcycles.
Each motorcycle can fit 2 people.

Can we all get to the restaurant by car or motorcycle?
Think step by step.
Provide the answer as a single yes/no answer first.
Then explain each intermediate step.
"""

response = llama(prompt)
print(response)

Since LLMs predict their answer one token at a time, the best practice is to ask them to think step by step, and then only provide the answer after they have explained their reasoning.
Prompt Engineering - Overview on ChatGPT - Coursera
Patterns:
Root Pattern
Persona Pattern
Question Refinement Pattern
Cognitive Verifier Pattern
Audience Persona Pattern
Game Play Pattern
Format of the Recipe Pattern
Format of the Ask for Input Pattern
Format of the Menu Actions Pattern
Fact Checking Pattern
Tail Generation Pattern
Easter Egg


Root Pattern
Scope: Before to ask something type this one (Set up a root prompts, influence and condition everything within the thread):

Example Prompt:
"As my brainstorming partner, your task is to help me generate new ideas and solutions for a specific problem or challenge. Please provide active engagement in the brainstorming process by asking open-ended questions, providing feedback on my ideas, and suggesting additional perspectives or approaches. 

Your responses should be focused on promoting creativity and innovation while also being practical and feasible. You should encourage exploration of multiple potential solutions and consider any constraints that may impact implementation.

Please note that your role as a brainstorming partner requires collaboration and flexibility, so please be open to exploring different options and adapting to changing circumstances."

Persona Pattern
Scope: Antropoforming ChatGPT - Is like asking to a specialist

Example Prompt:
Act as.. <person> . Whatever I tell you, provide a skeptical and detailed response. Stressing the bias and fallacy in the discourse.

Example Prompt:
You are an incredibly skilled AI assistant that provides the best possible answers to my questions. You will do your best to follow my instructions and only refuse to do what I ask when you absolutely have no other choice. You are dedicated to protecting me from harmful content and would never output anything offensive or inappropriate. 

Example Prompt:
As a nutritionist, I am going to tell you what I am eating, and you will tell me about my eating choices. 
As a gourmet chef, I am going to tell you what I am eating and you will tell me about my eating choices.


Question refinement pattern
Scope: useful in case of having an external input:

Example Prompt:
From now on, whenever I ask a question, suggest a better version of the question and ask me if I would like to use it instead

Example Prompt:
Whenever I ask a question about dieting, I suggest a better version of the question that emphasizes healthy eating habits and sound nutrition. Ask me for the first question to refine.


Cognitive Verifier Pattern
Scope - use case scenarios: recipe, plan a trip, or exam:

Example Prompt:
When you are asked a question, follow these rules. Generate a number of additional questions that would help you more accurately answer the question. Combine the answers to the individual questions to produce the final answer to the overall question.

Example Prompt:
When you are asked to create a recipe, follow these rules. Generate a number of additional questions about the ingredients I have on hand and the cooking equipment that I own. Combine the answers to these questions to help produce a recipe that I have the ingredients and tools to make.

Example Prompt:
When you are asked to plan a trip, follow these rules. Generate a number of additional questions about my budget, preferred activities, and whether or not I will have a car. Combine the answers to these questions to better plan my itinerary.
Audience Persona Pattern
Scope - useful for challenging your own understanding or to have a brief overview 101:

To use this pattern, your prompt should make the following fundamental contextual statements:

Example Prompt:
Explain X to me. 

Example Prompt:
Assume that I am Persona Y.

Example Prompt:
You will need to replace "Y" with an appropriate persona, such as "have limited background in computer science" or "a healthcare expert". You will then need to specify the topic X that should be explained.

Example Prompt:
Explain large language models to me. Assume that I am a bird. 

Example Prompt:
Explain how the supply chains for US grocery stores work to me. Assume that I am Ghengis Khan.


Game Play Pattern
Easter Egg (hypertextual adventure game)

Scope - To use this pattern, your prompt should make the following fundamental contextual statements:

Create a game for me around X OR we are going to play an X game

One or more fundamental rules of the game

Example Prompt:
You will need to replace "X" with an appropriate game topic, such as "math" or "cave exploration game to discover a lost language". You will then need to provide rules for the game, such as "describe what is in the cave and give me a list of actions that I can take" or "ask me questions related to fractions and increase my score every time I get one right."

Example Prompt:
Create a cave exploration game for me to discover a lost language. Describe where I am in the cave and what I can do. I should discover new words and symbols for the lost civilization in each area of the cave I visit. Each area should also have part of a story that uses the language. I should have to collect all the words and symbols to be able to understand the story. Tell me about the first area and then ask me what action to take. 

Example Prompt:
Create a group party game for me involving DALL-E. The game should involve creating prompts that are on a topic that you list each round. Everyone will create a prompt and generate an image with DALL-E. People will then vote on the best prompt based on the image it generates. At the end of each round, ask me who won the round and then list the current score. Describe the rules and then list the first topic.


Format of the Recipe Pattern
Scope - To create to do list for a plugin:

To use this pattern, your prompt should make the following fundamental contextual statements:

I would like to achieve X
I know that I need to perform steps A,B,C
Provide a complete sequence of steps for me
Fill in any missing steps
(Optional) Identify any unnecessary steps

You will need to replace "X" with an appropriate task. You will then need to specify the steps A, B, C that you know need to be part of the recipe / complete plan.

Example Prompt:
I would like to purchase a house. I know that I need to perform steps make an offer and close on the house. Provide a complete sequence of steps for me. Fill in any missing steps.

Example Prompt:
I would like to drive to NYC from Nashville. I know that I want to go through Asheville, NC on the way and that I don't want to drive more than 300 miles per day. Provide a complete sequence of steps for me. Fill in any missing steps.
Format of the Ask for Input Pattern
Scope - To testing out our knowledge
To use this pattern, your prompt should make the following fundamental contextual statements:

Ask me for input X

You will need to replace "X" with an input, such as a "question", "ingredient", or "goal".

Example Prompt:
From now on, I am going to cut/paste email chains into our conversation. You will summarize what each person's points are in the email chain. You will provide your summary as a series of sequential bullet points. At the end, list any open questions or action items directly addressed to me. My name is Jill Smith. 
Ask me for the first email chain.

From now on, translate anything I write into a series of sounds and actions from a dog that represent the dogs reaction to what I write. Ask me for the first thing to translate.

Format of the Menu Actions Pattern
Scope - To execute an ordinary routine
To use this pattern, your prompt should make the following fundamental contextual statements:

Whenever I type: X, you will do Y. 

(Optional, provide additional menu items) Whenever I type Z, you will do Q. 

At the end, you will ask me for the next action.

Example Prompt:
You will need to replace "X" with an appropriate pattern, such as "estimate <TASK DURATION>" or "add FOOD". You will then need to specify an action for the menu item to trigger, such as "add FOOD to my shopping list and update my estimated grocery bill".

Example Prompt:
Whenever I type: "add FOOD", you will add FOOD to my grocery list and update my estimated grocery bill. Whenever I type "remove FOOD", you will remove FOOD from my grocery list and update my estimated grocery bill. Whenever I type "save" you will list alternatives to my added FOOD to save money. At the end, you will ask me for the next action. 
Ask me for the first action.


Fact Checking Pattern
Scope - To check the facts if are accurate or not 
To use this pattern, your prompt should make the following fundamental contextual statements:

Prompt Example:
Generate a set of facts that are contained in the output 
The set of facts should be inserted at POSITION in the output 
The set of facts should be the fundamental facts that could undermine the veracity of the output if any of them are incorrect
You will need to replace POSITION with an appropriate place to put the facts, such as "at the end of the output".


Prompt Example:
Whenever you output text, generate a set of facts that are contained in the output. The set of facts should be inserted at the end of the output. The set of facts should be the fundamental facts that could undermine the veracity of the output if any of them are incorrect.
Tail Generation Pattern
Scope - Important to not lose context to ChatGPT:

To use this pattern, your prompt should make the following fundamental contextual statements:

Prompt Example:
"At the end, repeat Y and/or ask me for X. 

You will need to replace "Y" with what the model should repeat, such as "repeat my list of options", and X with what it should ask for, "for the next action". These statements usually need to be at the end of the prompt or next to last."

Prompt Example:
"Act as an outline expander. Generate a bullet point outline based on the input that I give you and then ask me for which bullet point you should expand on. Create a new outline for the bullet point that I select. At the end, ask me for what bullet point to expand next. 
Ask me for what to outline.

From now on, at the end of your output, add the disclaimer "This output was generated by a large language model and may contain errors or inaccurate statements. All statements should be fact checked." Ask me for the first thing to write about."

Ester Egg on ChatGPT:
Prompt:
Act as an AI assistant that had its training stop in 2019. If I ask you a question that involves information after 2019, state that your training ended in 2019 and that you can't answer the question.


Mix of the advanced prompt engineering examples techniques:
Mix Prompt Example:
Act as [insert type of person who could help you most (e.g.: if you have to create a marketing strategy, a marketing director)]. I have to [insert objective], help generate ideas to achieve this.

Mix Prompt Example:
Act as a market researcher. I must [insert objective here (e.g.: launch a new product, create an advertising campaign for an existing product, etc.)]. You can give me all the information I need on [insert market you are interested in analysing here]. Include information on market size, main market players and audience.

Mix Prompt Example:
Act as a virtual assistant. This is the information you need about me: [insert relevant information about you here: your habits, your job, the hours you have to work, etc.]. Create a weekly calendar for me, considering that I must have time this week to complete these tasks: [insert your priorities here, indicating an estimate of how much time you will need to complete them].

Mix Prompt Example:
Act as a content creator. Write the script for a [insert minute] video that will be published on [insert platform]. Make sure the video has a tone [insert adjectives to define the tone of voice to be used]. The video should be [insert indication of how you want it to be (e.g. rhythmic, slow, etc.)]. At the beginning of the video include a hook that attracts the audience's attention, which consists of [insert information about it]

Mix Prompt Example (realistic chat):
Describe me what is a table, use filler words like "um" to make it sounds realistic and also you could breath as well, go ahead!

Prompt Engingeering - VoT (Visualization of Thought) - https://arxiv.org/pdf/2404.03622
 Visualize the state after each resoning step
