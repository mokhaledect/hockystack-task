# Process Meetings

## Code Quality and Readability Enhancements

The code is written in an imperative way or style which makes the code is un-readable and so hard to maintain. Also using JS as a dynamic language makes it harder.
As for quality, code is not organized. single file has everything there is not a good practice at all. 

### Suggestions 
    1- Use a Declarative way or style of development (find samples down).
    2- Apply typescript to enforce types to the code, this will simplify maintenance and trace issues in easier way.
    3- Use more abstracted Units to encapsulate parts of logic (find samples down)

## Project Architecture Enhancements
The project currently doesn't follow any known architectures.

### Suggestions 
We can apply one of these architectures 
    
    1- Modular Monolith 
        * This architecture supports having extending the project to have multiple modules each module is isolated and can be at any time separated and run individually.
          in this case each module is considered a Processor that process some data for example (Meetings). The meeting Processor can at any time isolated and separated on its own service and run individually.

    2- Entity Boundary Interactor (EBI) = (Screaming Features)
        * if each processor is not meant to be extended in terms of logic then we can follow this architecture as a list of features managed by an orchestrator that controls which feature(s) to execute.
        we going to have project structure like this 
        Project: 
            features:
                process-contacts
                    handler
                process-companies
                    handler
                process-meetings
            orchestrators:
                worker.js => setting up the queue or receive it in the constructor. execute features handlers.

## Code Performance
As we manages a lot of data we want embrace parallelism an asynchronous execution of data pipelines.

### Suggestions
    We want to execute in parallel the different processors. 
    await processCompanies();
    await processContacts();
    await processMeetings();

    it is not a good solution. and it is blocking main node thread. we can execute each processor on its own worker.

    we can use Message Brokers to enhance data consuming and execution in general but we gonna need setup more sophisticated architecture for it.


## Samples

### Abstracted Units

    class CompanyUtility {
        actionFromCompany(company) {
            return action;
        }
    }

    class CompaniesProcessor {
        public handle(companies) {
            uses CompanyUtility

            const action = CompanyUtility.actionFromCompany(company);  // declarative way of programming

            q.push(action);
        }
    }

    class Executor {
        x = new CompaniesProcessor();
        y = new ContactsProcessor();
        z = new MeetingsProcessor();

        loop start

            try in parallel
            result = await Promise.AllSettled([x.handle(), y.handle(), z.handle()]);

        loop again.
    }
