# Backend Developer Challenge

Hi there!

We are excited about you considering to join our small team of dedicated engineers.

Below you will find a technical challenge that will help us understand your ability to write clean concise code, which is easy to reason about, maintain and support. We are also keen on seeing your approach to naming (because we know how hard naming is!) and structuring the entities around your codebase, as well as testing and documenting them.

Good luck, and looking forward to welcome you aboard.

## Spec

Implement a RESTful in-memory discovery service.

The idea is simple: a number of different client applications will periodically send heartbeats to this service,
and the service keeps track of them, periodically removing those that didn't send any heartbeats in some configured time frame.

### Endpoints

The service should expose the following endpoints:

- `POST /:group/:id`

    - registers an application instance in specified `group`
    - `id` is a unique identifier of application instance, generated by client
    - if instance with `id` is already registered, the `updatedAt` timestamp must be updated
    - the request body must be in JSON format and can specify meta information that will be attached to the instance
    - returns JSON with following structure:

        ```json
        {
            "id": "e335175a-eace-4a74-b99c-c6466b6afadd", // echoed from path parameter
            "group": "particle-detector", // echoed from path parameter
            "createdAt": "1571418096158", // timestamp of the first registered heartbeat of this instance
            "updatedAt": "1571418124127", // timestamp of the last registered heartbeat of this instance
            "meta": {   // echoed from request body
                "foo": 1
            }
        }
        ```

- `DELETE /:group/:id`

    — unregisters an application instance
    - does not accept request body
    - returns 204 with no content, even if instance does not exist

- `GET /`

    - returns a JSON array containing a summary of all currently registered groups as follows:

        ```json
        [
            {
                "group": "particle-detector",
                "instances": 4, // the number of registered instances in this group
                "createdAt": "1571418124127", // the timestamp of the first heartbeat registered in this group
                "lastUpdatedAt": "1571418124127", // the timestamp of the last heartbeat registerd in this group
            },
            // ...
        ]
        ```

    - groups containing 0 instances should not be returned

- `GET /:group`

    - returns a JSON array describing instances of the `group`:

        ```json
        [
            {
                "id": "e335175a-eace-4a74-b99c-c6466b6afadd",
                "group": "particle-detector",
                "createdAt": "1571418096158",
                "updatedAt": "1571418124127",
                "meta": {
                    "foo": 1
                }
            },
            // ...
        ]
        ```

### Background task

The service should include a background task that will periodically remove expired instances. The "age" of the most recent heartbeat of an instance to be considered expired should be configurable with environment variable and have a sensible default value.

## Our expectations

- A service should be implemented in Node.js, with README containing steps to get it up and running.
- TypeScript with `strict: true` is strongly encouraged, but not 100% required.
- Unless something is explicitly stated in Spec, you are free to choose how to approach the specifics. Don't forget to document important implementation decisions.
    - Feel free to challenge the spec if something does not add up, or can be done more elegantly and/or efficiently. We appreciate the ability to work with the requirements.
- This is a fairly simple task, so it should not take more than few ours for you to implement the base functionality. However, we expect you to spend some time with polishing, refactoring and testing.
- Try to limit the usage of "utility belts" (underscore, lodash, ramda); instead prefer latest ECMAScript features.
- Feel free to use latest Node version (we don't like legacy either!)

## Submitting solution

- Create a repository on GitHub and/or BitBucket, push your code there and send us a link.
- If you choose a private repository (e.g. on BitBucket), request the username of one of our engineers so that you could add them as collaborators.
- Alternatively, download your private repository as ZIP and attach it to the email.
- Deploying the solution to some public service like Glitch so that we could test it without cloning locally would be a huge plus (we appreciate you appreciating the time of our engineers!) — but not required.
- Showing a green CI page would also be beneficial — and also not required.