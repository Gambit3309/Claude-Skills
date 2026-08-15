**1. Name of the Skill**



n8n-workflow-advisor-SKILL



**2. Purpose**



i built this because most n8n help either just hands you a finished workflow or a JSON blob to import. i didn't want that. i wanted something to actually think through automation ideas with — ask me questions, push back if a design has a weak spot, explain why one approach beats another instead of just telling me what to click. basically a thinking partner, not a workflow generator.



**3. How It Works**



you give it one of three things: a vague idea ("automate my invoicing"), an existing n8n workflow (pasted JSON or just described), or a specific well-scoped request ("trigger on X, do Y").



vague idea → it doesn't jump to one answer. it gives you 2-3 different approaches with the trigger, rough flow, and trade-off for each, then asks which direction fits before going deeper

existing workflow → it always asks what you actually want first (debug it, extend it, critique it) instead of guessing, since the feedback looks totally different depending on the goal

specific request → it goes straight to a node-by-node breakdown, but every node comes with a "why this, not that," plus open questions and likely failure points at the end



it stays out of Docker/deployment stuff on purpose — that's a separate concern, this is just for the design conversation.



**4. Benefits**



for me, it means i'm not just copy-pasting a workflow i don't fully understand — i actually know why each node is there, which makes debugging way easier down the line. for anyone else using it, same thing: it forces you to think through trade-offs (simplicity vs robustness, trigger choice, failure handling) instead of defaulting to whatever's fastest to set up. and since it always asks before assuming, you don't end up with advice aimed at the wrong goal.

