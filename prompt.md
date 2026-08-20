let's make initialization for this project.

Task:
- analyze the project structure
- analyze how to run this project locally
- analyze how does this project deployed project trough github actions
- analyze how does the templating works
- analyze how does the branch management works
- analyze how does the cv generation works
- analyze the cv structure
- learn how to fetch data from linkedin using MCP browser. the linkedin profile URL is refered from index.md file.
- make project context in markdown format used for Gemini or Claude to store the knowledge and tracking the project progress/changes

Output:
- project context in markdown format, it must be compact and easy to understand
- make a custom command for this project to add new section to update the cv. user can the information by themself or fetch from linkedin. it must be interactive.
- make a custom command to generate a new template for cv. it must be interactive. user can specify the template reference using URL or description.

---

lets evaluate results you made:
1. QUICK_REFERENCE.md is still too long. make it compact and easy to understand. just keep the most important information.
2. CV data source only come from index.md file. don't make a workflow to create anoter new markdown. every new created workflow must focus to update index.md file (unless its for create a new template).
3. For add-cv-section command, linkedin url can be obtained from index.md file or user can input it manually.
4. update another markdown context after this update.

--- 

awesome! lets continue to add additional information to this project context:
- specific only for this local development, prevent user to commit directly to page-release branch or main branch. User must create a new branch and create a pull request to merge to page-release or main branch. provide workflow to create a new branch and create a pull request, if new branch is created, user can just make a pull request to merge to page-release or main branch. check if pull request already exists before creating a new pull request. if pull request already exists, user can update the pull request. 
- for all project context markdown file, add optional local development option to run ruby server and preview the cv directly instead of using docker compose. but by default, it must use docker compose to preview the cv locally.
- add all of created workflows to gemini cli so user can use it as custom command.


---

lets add additional information to this project context so you can modift README.md file for this project accordingly:
1. for development of this project, our practices is never commit directly to `main` or `page-release`. Always use feature branches and pull requests. But, for user who fork this repo, they can commit directly to `main` or `page-release` since they not required to raise PR to our branch. rollback your changes in README.md that related about this usecase.

---

lets make a plan for a new context workflow for our project. 

Goal: 
Make CV Build wizard workflow: Help the user construct the CV by curating several resources provided from user.

Description:
- The user will provide a list of sites that they have as the CV data sources. It can be a link or a file. AI Agent will check the link content using CURL or Browser MCP (if installed/available). If this link isn't accessible after several trials, the AI Agent will ask the user alternative link or file as a substitute for that information. If the user doesn't provide anything, the AI Agent will help the user by asking this question one by one.
- Use this skill as a reference for the construction or validation of the CV https://github.com/ComposioHQ/awesome-claude-skills/blob/master/tailored-resume-generator/SKILL.md. You can make a separate project context file to construct this if needed.
- Make sure to use syntax that is compatible with our project. After the generation is complete, do an iteration to validate the syntax and do the render testing.
- Ask the user about the order of each section. By default, use the unmodified @index.md from the main repo as the structure reference. 


Goal:
- Make CV Evaluation workflow: Provide fair critique and suggestions for the user's CV on @index.md based on user evaluation/improvement criteria. So the user can improve their CV structure/description, but the content is still personalized for them. 

Description:
- The user will describe what evaluation/improvement they need. User provides whether they want to evaluate all of the CVs or just a particular section. The user can provide the Job target they want to apply so the AI Agent can have another evaluation point or reference to give more targeted evaluation/improvement results. If the user didn't describe anything when running this workflow, the AI Agent will help the user by asking this question one by one.
- If needed. You can make a separate context document to make an analysis framework. Let's make an analysis framework to evaluate user's CV is match with his career goal/profile or the job that the user wants to applied. Construct the category of evaluation and the scoring mechanism to make sure the user can understand what kind of category or section they need to improve.
- Provide results in 2 ways: narration and score based on our analysis framework. optionally, ask the user if they want to generate the evaluation result as an HTML-based web view. If the user says yes, generate an interactive web dashboard that shows the evaluation results in neo-brutalism theme. At the end of the web section lets give a personal touch, like a quote, to give the user motivation and signature this web view generated by the AI Agent model name, and "brought to you...." wording, add this repository name and the repository owner username (link to GitHub profile and link to GitHub project). The AI Agent will provide helpful output to tell this web dashboard theme optionally can be customized after the generation is complete 
- If the user has already generate evaluation result as an HTML-based web view before, the AI Agent will ask if the user wants to replace the dashboard or wants to make it a new file. 

Let's make sure our workflow follows Gemini workflow best practice and follows anthropic skill best practice (https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf?hsLang=en). This skill can be universally used by any kind of AI Model.