## Hi there 👋 Welcome to Dossierr your GPT powered Legal Assistant
Dossierr helps you answer legal questions, either from the law, previous rulings or your own files. The site is a SAAS platform with various moving parts and a lot of code. The code is documented here on Github for everyone to see and validate. 

# Technical overview
Dossierr consists of a handful of docker containers working in unison to process legal data. Client request go through our NGINX service and end up at our Django Web application for processing. There are several other containers running to ensure the AI works, that legal data is scraped continuesly and that all processed information is stored in a way that can easily be retrieved. Lastly we have a task processor so long running and slow tasks can be dealt with in the background. 

![Dossierr structure](https://github.com/Dossierr/.github/assets/71013416/8fe21b74-15b5-41af-a99b-a86bee02ce92)

### Typical journey of a file in Dossierr:

##### Preparing information
1. A file is uploaded by the user on the frontend, and handled by our Django web framework
2. Django ensures that a database entry is recorded of the file and that it is stored into a S3 bucket that belongs to the end user
3. This triggers a background task as a new file means that we will need to update our vector database. The vector database contains snippits of text that can be quickly retrieved to complete a prompt.
4. Our background worker and GPT engine split all files of the user in chunks of text (with a overlap) and converts those into Vectors. Those are then added to Redis, ready for use.
  * This process also runs for each file scraped by our Law Scraper - which fetches court verdicts and changes to civil code. 
##### Answering a query
5. If the user asks a query in the chat prompt that query is also embedded into a vector. Through the process of a similarity search we can retrieve the text by looking wich chunk of text is most relevant to the query entered by the user
6. The user query, chat history and the relevant texts that were found in the database are sent to OpenAI and a answer is generated
7. Both the answer and query are added to the chat history, so the AI can keep track of what has been previously said. 

# License, Open Source and Creative Commons
The collection of works are published on Github and under the BY-NC-ND creative commons license.  This is because we value transparency, as well as want others to benefit from our work. This means you'll be able to host your own version of the code we developed. Restrictions do apply: we do not permit: using the works for commercial purposes, redistribution and modification. If you do want to use the code commercially please reach out for a license agreement.



### How does Creative commons work?
<img src="https://user-images.githubusercontent.com/71013416/230730932-b32e5048-5d7f-4f81-9df1-bfc658f6f5e4.png" width="100">

It's very simple, it is like when you tell someone the magic ingredient of a recipe. Some people choose not to tell what is in the recipe (like coca-cola) others tell what's in it but ask you not to make the same product and there may even be people that will not put any restrictions on how you use the recipe.

For Dossierr: we chose to tell everyone how our product works under the following conditions:
- You mention that it is our work, and not yours
- You cannot use our work for commercial purposes. If you want to though, as we can work out a license agreement
- If you use our work as a basis and make changes then we do not allow you to distribute that work - So you can make any change for your own use. If you think you can improve the code, then open a pull request.
For the exact stipulations check our Creative Commons license.
