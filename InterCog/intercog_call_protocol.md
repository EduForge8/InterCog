# InterCog Plugin Call Protocol

To use InterCog for interdisciplinary reasoning, follow this protocol:

1. When a user asks a complex question requiring cross-domain reasoning,
   send a POST request to the InterCog API:

   POST http://localhost:8000/intercog/analyze

2. The request body must follow this schema:

{
  "query": "<user question>",
  "user_profile": {},
  "context": {},
  "options": {}
}

3. InterCog will return a structured JSON response containing:
   - scenario
   - disciplines_used
   - cross_domain_bridge
   - output
   - limitations
   - compliance

4. Render the "output" field as natural language to the user.

5. Always include "limitations" and "compliance" in the final answer.
