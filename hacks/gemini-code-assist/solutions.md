# Gemini Code Assist

## Introduction

Welcome to the coach's guide for Gemini Code Assist gHack. Here you will find links to specific guidance for coaches for each of the challenges.

> **Note** If you are a gHacks participant, this is the answer guide. Don't cheat yourself by looking at this guide during the hack!

## Coach's Guides

- Challenge 1: Clone the Quotes app
  - Get started with your GCP project and clone the repository to edit within your environment.
- Challenge 2: Get started with Gemini Code Assist
  - Experiment with prompt engineering to understand the Gemini Code Assist feature.
- Challenge 3: Test-Driven Development
  - Have Gemini help you to add business logic using test-driven development guidelines.
- Challenge 4: Build, deploy, and test
  - Build and deploy the updated Quotes app to Cloud Run and test the endpoint.
- Challenge 5: Enhancing the Quotes app with GenAI
  - Use Google's GenAI capabilities to enhance the application.

## Google Cloud Requirements

This hack requires students to have access to Google Cloud project where they can create and consume Google Cloud resources. These requirements should be shared with a stakeholder in the organization that will be providing the Google Cloud project that will be used by the students.

- Access to Gemini Code Assist within their Google Cloud Project
- Permission to use and create Cloud Build and Cloud Run resources

## Challenge 1: Clone the Quotes App

### Notes & Guidance

The below guidelines assume you are using Cloud Shell within the GCP console to run this lab. If you are using your own machine, you will likely need to use the [gcloud package](https://cloud.google.com/sdk/docs/install) for your local terminal instead.

#### Activate Cloud Shell

- Cloud Shell is a virtual machine that is loaded with development tools. It offers a persistent 5GB home directory and runs on the Google Cloud. Cloud Shell provides command-line access to your Google Cloud resources.
- In the Cloud Console, in the top right toolbar, click the Activate Cloud Shell button.
  ![Activate Cloud Shell](https://github.com/GoogleCloudPlatform/cloud-hackathons/assets/73710075/a914437c-aa75-4ce7-be29-931c962c02aa)
- Click Continue.
  ![Continue](https://github.com/GoogleCloudPlatform/cloud-hackathons/assets/73710075/69a782f3-6506-400e-93ff-a0e15893f040)

It takes a few moments to provision and connect to the environment. When you are connected, you are already authenticated, and the project is set to your PROJECT_ID. For example:
![PROJECT_ID](https://github.com/GoogleCloudPlatform/cloud-hackathons/assets/73710075/b3ca7217-884d-44ec-9153-eacd8431cf5d)

#### Enable Gemini Code Assist API

Start by obtaining new credentials, by typing the following command, authorizing the request at the shared link, then copy/paste the authorization code in the Terminal :

```shell
gcloud auth login
```

Enable the APIs required by the Quotes application:

```shell
#enable the Gemini Code Assist API
gcloud services enable cloudaicompanion.googleapis.com
```

To use Gemini Code Assist in the Console, click on the button in the navigation bar to open the chat panel:
![UI](https://github.com/GoogleCloudPlatform/cloud-hackathons/assets/73710075/6d9fef08-ca0f-4874-b255-c27a117cc16b)

Click "Start Chatting" and try a few queries about GCP or general programming related topics.

#### Open the Editor

As you are opening it in an incognito window, the editor will ask you to Open In a new window

![UI](https://github.com/GoogleCloudPlatform/cloud-hackathons/assets/73710075/ee2ee1b9-0a3b-485a-ae27-902ae8bdbda2)

#### Enabling Gemini Code Assist in the Cloud Shell Editor

Gemini Code Assist is available in a range of Editors. For the full list check the official documentation.
In this lab we will use the Cloud Shell Editor but the instructions for enabling Gemini Code Assist are the same for all VS-Code based editors.
Once the editor has loaded, make sure you have the Cloud Code Extension installed. For Cloud Shell Editor this extension is automatically installed for you. If you were to use your own IDE you might have to install it manually.
Open the settings page via Menu > File > Preferences > Settings by using the shortcut of CTRL+, (CMD+, for macOS).

In the settings page search for "Gemini Code Assist" and check the boxes for using Gemini Code Assist and to enable suggestions.

Lastly, you'll need to set the GCP project that Gemini Code Assist will be using. We'll use the project that you were given in the Qwiklabs instructions page. Don't worry about saving the changes. They are automatically saved on edit.

![UI](https://github.com/GoogleCloudPlatform/cloud-hackathons/assets/73710075/36b5c1bd-08e3-44a9-acea-98ba82d0cdf5)

At the bottom left of the editor you'll see a button with the label "Cloud Code - Sign in". Click to login to the lab's GCP account. After a few seconds of clicking the button you should see the label change to "Cloud Code - No Project".

![UI](https://github.com/GoogleCloudPlatform/cloud-hackathons/assets/73710075/092bb7d3-22a1-4c63-a47d-93bc4618077b)

Install the latest Java version - Java 21 with the SDKMan Software Developer Kit Manager, for the simplest installation:

```shell
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

Install the latest OpenJDK and confirm it as the default JDK in the install

```shell
#install OpenJDK
sdk install java 21.0.2-tem && sdk use java 21.0.2-tem && java -version
```

Enable the APIs required by the Quotes application, if they are not already activated:

```shell
#enable the Gemini, Cloud Run, Cloud Build and Logging APIs

gcloud services enable cloudaicompanion.googleapis.com
gcloud services enable cloudbuild.googleapis.com 
gcloud services enable run.googleapis.com
gcloud services enable logging.googleapis.com 
```

Run the following steps in Cloud Shell window activated in Task 1 to create and build the application:

First, let's get the codebase for Quotes by cloning the Github repo and switching to the /services/quotes folder:

```shell
# clone the repo
git clone https://github.com/GoogleCloudPlatform/serverless-production-readiness-java-gcp.git
cd serverless-production-readiness-java-gcp/services/quotes
```

Open the codebase in the IDE, by adding the serverless-production-readiness-java-gcp/services/quotes folder to the Workspace:

VSCode suggests to add a Java Extension pack; enable it, as it is the only recommendation which you would find useful in this lab; you can ignore the others.

Observe that the Java Project is being opened, this takes a few seconds.

Explore the codebase by clicking on the Explorer, the top-left icon.

After opening the code in the Explorer, you might observe that the Gemini button is disabled.
Repeat the previous sign in step:  at the bottom left of the editor you'll see a button with the label "Cloud Code - Sign in". Click to login to the lab's GCP account. After a few seconds of clicking the button you should see the label change to "Cloud Code - No Project" and Duet AI is enabled.

If the terminal window is closed, re-open it and set the project as in the previous step.

```shell
#set the project
gcloud config set project <your project ID>
```

Then set the PROJECT_ID environment variable:

```shell
#set the PROJECT_ID env variable
export PROJECT_ID=$(gcloud config list --format 'value(core.project)')
echo   $PROJECT_ID
```

Check that the project is set:

```shell
#validate it with the command
gcloud config list
```

Validate that you have Java 21 and Maven installed:

```shell
sdk use java 21.0.2-tem && java -version
```

Validate that the starter app is good to go:

```shell
./mvnw package spring-boot:run
```

Open a new terminal window. From the terminal window, test the app:

```shell
curl localhost:8083/start -w "\n"
```

#### Output

QuoteController started!
In the terminal window where the app is running, Stop the running app using CTRL+C
Alternatively, you can start the Quotes app also by using plain Java:

```shell
java -jar target/quotes-1.0.0.jar 
```

Build a JIT Docker image with Dockerfiles:

```shell
# build an image with a fat JAR
docker build -f ./containerize/Dockerfile-fatjar -t quotes .

# build an image with custom layers
docker build -f ./containerize/Dockerfile-custom -t quotes-custom .
```

Build a Java Docker image with Buildpacks:

```shell
./mvnw spring-boot:build-image -DskipTests -Dspring-boot.build-image.imageName=quotes
```

Test the locally built images on the local machine from a terminal window:

```shell
docker run --rm -p 8080:8083 quotes

#Test the start endpoint
curl localhost:8080/start -w "\n"
```

Stop the running Docker container with CTRL+C

## Challenge 2: Get started with Gemini Code Assist

This challenge is fairly open ended and is meant to provide students with time to get familiar with Gemini Code Assist in their IDE as well as prompt design. The instructions will provide most guidelines for this challenge, but here are some example prompts:

Open the `QuoteApplication` class (CMD+P on the Mac), then type the following prompt in the Gemini Code Assist chat window and observe the response:

```text
I want to get details about the QuotesApplication; please provide a detailed overview of the QuotesApplication
```

Open the `QuoteController` class, then type the the following prompt in the Gemini Code Assist chat window:

```text
Please perform a detailed code review of the QuoteController
```

You can ask Gemini Code Assist for improvements to the class after the code review:

```text
Please recommend code improvements to the QuoteController class
```

The `NullPointerException` identified in the challenge notes can be thrown due to there being no check that a quote exists in the `updateQuote` method. Students can ask Gemini Code Assist for fixes to this issue, but one example would be the following:

```java
    @PutMapping("/quotes/{id}")
    public ResponseEntity<Quote> updateQuote(@PathVariable("id") Long id, @RequestBody Quote quote) {
        try {
            Optional<Quote> existingQuote = quoteService.findById(id);
            
            if(existingQuote.isPresent()){
                Quote updatedQuote = existingQuote.get();
                updatedQuote.setAuthor(quote.getAuthor());
                updatedQuote.setQuote(quote.getQuote());
                quoteService.updateQuote(updatedQuote);

                return new ResponseEntity<Quote>(updatedQuote, HttpStatus.OK);
            } else {
                return new ResponseEntity<Quote>(HttpStatus.NOT_FOUND);
            }
        } catch (Exception e) {
            System.out.println(e.getMessage());
            return new ResponseEntity<Quote>(HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }
```

## Challenge 3: Test Driven Development

With a solid starting point for the Quotes app, it is time to build new business functionality using a test-driven development process assisted by Gemini Code Assist.

The *Quotes* app is missing an endpoint to retrieve book information by book name.
This endpoint should respond on the `/quotes/book/{book}` path. You are being asked to implement it, with the associated business logic.

Let's use a test-driven approach to add this functionality, starting from writing an application test.

Start by asking Gemini Code Assist to recommend which types of tests you need to write:

```text
Which types of test should I be writing for the QuoteController
```

Gemini Code Assist will reply with a list including:

- Unit tests
- Integration tests
- System tests
- Acceptance tests

Open the `/src/test/java/com/example/quotes` folder and observe that several test classes have already been created:

- QuotesControllerTest
- QuotesRepositoryTest
- QuotesApplicationNetworkFailuresTests

To add the quote retrieval by book name functionality, you start writing code in true TDD fashion by adding tests to both the `QuotesControllerTest` (for the endpoint) and `QuotesRepositoryTest` (for data retrieval from the db).

### Step 1: Generate test first

Open the `QuotesControllerTest` class in the `com.example.quotes.web` package
In the code,and add the comment, say towards the end of the file and press Enter

```java
// Answer as a Software Engineer with expertise in Java. Create a test for the QuotesController for a method getByBook which responds at the HTTP endpoint /quotes/book/{book} and retrieves a quote from the book The Road
```

You can accept the suggestion, if it meets your requirements, with Tab or click Accept.

In the Terminal window, run  the command:

```shell
./mvnw clean verify
```

Observe that the test fails, as expected, with a '404' error, as the business logic has not been implemented:

```shell
[ERROR] Failures: 
[ERROR]   QuotesControllerTest.shouldReturnQuoteByBook:94 Status expected:<200> but was:<404>
[INFO] 
[ERROR] Tests run: 15, Failures: 1, Errors: 0, Skipped: 0
```

### Step 2: Generate controller code

Let's add the missing controller method getByBook. Open the `QuoteController` class.
Add the following comment towards the end of the class:

```java
// generate a getByBook method which responds at the HTTP endpoint /quotes/book/{book} and retrieves a quote by book name; use the QuoteService class to retrieve the book by name, as a String
```

Gemini Code Assist will respond with a code block along the lines of:

```java
    @GetMapping("/quotes/book/{book}")
    public ResponseEntity<List<Quote>> quoteByBook(@PathVariable("book") 
                String book) {
        try {
            List<Quote> quotes = quoteService.getByBook(book);

            if(!quotes.isEmpty()){
                return new  ResponseEntity<List<Quote>>(quotes, 
                                                        HttpStatus.OK);
            } else {
                return 
                   new ResponseEntity<List<Quote>>(HttpStatus.NOT_FOUND);
            }
        } catch (Exception e) {
            System.out.println(e.getMessage());
            return 
          new ResponseEntity<List<Quote>>(HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }
```

Note that the code is missing the `getByBook()` implementation in the QuoteService class, which leads you to the next step in the implementation.

### Step 3: Generate now repository access methods, starting again with a test

Open the QuoteService class and observe that the getByBook method is missing.
Let's generate a test for the service class, add methods to access the repository, then test it out

Open the `QuotesRepositoryTest` class and add the following comment towards the bottom of the class:

```java
// generate a unit test for the getByBook method in the QuoteService; create a Quote in the QuoteService first then test the getByBook method against the new Quote
```

Observe that the generated code looks like:

```java
  @Test
  @DisplayName("Get a quote by book - failed")
  void testGetQuoteByBookFailed(){
    var quotes = this.quoteService.getByBook("The Glass Menagerie");
    assertThat(quotes).isEmpty();
  }
```

With the tests implemented, it is time to implement the missing functionality in the `QuoteRepository` and the `QuoteService` class.

You want to get a `Quote` by the book name, therefore you need to add a `findByBook` method to the JPA repository class `QuoteRepository`, then allow the `QuoteService` to use this method

Open the `QuoteRepository` class and add this comment towards the end of the class:

```java
// generate a find by book method which retrieves a quote by book name; use the native query syntax
```

Gemini Code Assist will generate code along the lines of:

```java
  @Query( nativeQuery = true, value =
            "SELECT id,quote,author,book FROM quotes WHERE book = :book")
  List<Quote> findByBook(String book);

 ```

With the repository method in place, generate the missing link, the getByBook method in the service class and test it out. Open the QuoteService class and add the comment:

```java
// add get by book method, use the QuoteRepository
```

Gemini Code Assist will generate code along the lines of:

```java
  public List<Quote> getByBook(String book) {
    return quoteRepository.findByBook(book);
  }
```

Done! With coding, let's test the result.

- Right-click in the `QuotesRepositoryTest` and 'Run Tests'
- Right-click in the `QuotesControllerTest` class and 'Run Tests'

Run ALL tests from the Terminal:

```shell
./mvnw verify
```

Ask Gemini Code Assist to generate cURL commands to test the newly added functionality:

Start the app with

```shell
./mvnw spring-boot:run
```

Switch to a different terminal window to run a cURL command.
In the Gemini Code Assist AI chat window, prompt Code Assist to generate a test command

```text
generate a curl command for the /quotes/book endpoint for a local environment at port 8083 for the book "The Road"
```

Gemini Code Assist will generate the cURL command, which you can run:

```shell
curl -X GET http://localhost:8083/quotes/book/The%20Lord%20of%20the%20Rings
```

Assume the command has not found a book and we wish to print the HTTP error code; prompt Gemini Code Assist with:

```text
update the curl command to print the HTTP response code
```

Run the updated command generated by Gemini Code Assist, which should return a 404:

```shell
curl -X GET http://localhost:8083/quotes/book/The%20Lord%20of%20the%20Rings -o /dev/null -s -w '%{http_code}\n'
```

Now update the prompt to generate a successful command:

```text
update the command again to use the book "The Road"
```

Run the updated command generated by Gemini Code Assist, which should return a 404:

```shell
curl -X GET http://localhost:8083/quotes/book/The%20Road -o /dev/null -s -w '%{http_code}\n'
```

## Challenge 4: Build, deploy, and test

Before you can use Cloud Build, you'll need to update the YAML file in the repository. You can ask Gemini to generate the build-project step: below is the solution.

```yaml
- id: 'build-project'
name: maven:3.9.5-eclipse-temurin-21
  entrypoint: mvn
  volumes:
  - name: 'maven-repository'
    path: '/root/.m2'
    args: ["spring-boot:build-image", "-DskipTests", "-Dspring-boot.build-image.imageName=quotes"]

```

There are two options to build, tag and push the image to a container registry: a step-by-step manual commands approach and a more effective one using CloudBuild.

Option 5.1 Build, tag, push the image from the command line
Build a Java Docker image:

```shell
./mvnw spring-boot:build-image -Dspring-boot.build-image.imageName=quotes
```

Check that the `PROJECT_ID` is set in your terminal:

```shell
export PROJECT_ID=$(gcloud config list --format 'value(core.project)')
echo   $PROJECT_ID
```

Configure Docker Auth to be able to push the image to the Container Registry:

```shell
# get credentials to push to Google Container Registry
gcloud auth configure-docker
```

If you have built the image locally, tag it first and push to a container registry:

```shell
# tag
docker tag quotes gcr.io/${PROJECT_ID}/quotes
```

Push to a container registry:

```shell
# push Java image
docker push gcr.io/${PROJECT_ID}/quotes
```

Option 5.2 Build image using Cloud Build

Cloud Build supports a simple build, tag, push process in a single YAML file.
Start by validating that your PROJECT_ID is set:

```shell
# tag the image
export PROJECT_ID=$(gcloud config list --format 'value(core.project)')
echo   $PROJECT_ID
```

Build the Quotes application image using:

```shell
gcloud builds submit  --machine-type E2-HIGHCPU-32
```

Deploy the built image
Check existing deployed Cloud Run Services:

```shell
export PROJECT_ID=$(gcloud config list --format 'value(core.project)')
echo   $PROJECT_ID

gcloud run services list
```

Deploy the Quotes JIT image:

```shell
# note the URL of the service at the end of deployment
gcloud run deploy quotes \
     --image gcr.io/${PROJECT_ID}/quotes \
     --region us-central1 \
     --memory 2Gi --allow-unauthenticated

```

First, list the quotes cloud run service:

```shell
# find the Quotes URL is you have not noted it
gcloud run services list | grep quotes

✔  quotes                    us-central1   https://quotes-...-uc.a.run.app       

# validate that app passes start-up check

```

Update the URL in the following command, and run it:

```shell
URL= ...assign to URL you have retrieved from “gcloud run services list | grep quotes”

curl $URL:/start
# get quotes from the app
curl $URL:/quotes
curl $URL:/random-quote
curl $URL:/quotes/book/The%20Lord%20of%20the%20Rings 

# add a new quote to the repository
curl --location '$URL:/quotes' \
--header 'Content-Type: application/json' \
--data '{
    "author" : "Isabel Allende",
    "quote" : "The longer I live, the more uninformed I feel. Only the young have an explanation for everything.",
    "book" : "City of the Beasts"
}'
```

If you have deployed the app with security enabled, (no --allow-unauthenticated flag) you can test it with a Bearer token. You can also use as an alternative HTTPie (HTTP test client). Set the value of the `URL` variable in the following command, and run it:

```shell
TOKEN=$(gcloud auth print-identity-token)
URL=...assign to URL you have retrieved from
gcloud run services list | grep quotes

# Get the URL of the deployed app
# Test JIT image
curl -H "Authorization: Bearer $TOKEN" https://$URL/random-quote
curl -H "Authorization: Bearer $TOKEN" https://$URL/quotes

```

## Challenge 5: Adding GenAI

The final challenge will be to update the *Quotes* app using Generative AI. We can use the Gemini API to request that new quotes be generated and printed to the command line in a few simple steps. We'll start out by identifying our project ID, region, and model ID to set as environment variables.

```shell
export VERTEX_AI_GEMINI_PROJECT_ID=<your project id>
export VERTEX_AI_GEMINI_LOCATION=<region>
export VERTEX_AI_GEMINI_MODEL=<<model id>>
```

Next, open the `GenerateQuote` class in the domain folder and use Gemini to generate the rest of the method to be filled in. You should develop something similar to the following:

```java
public Quote findRandomQuote() {
  ChatResponse chatResponse = chatClient.call(new Prompt("Give me a quote from a classic book... ",
    VertexAiGeminiChatOptions.builder()
      .withTemperature(0.4f)
      .build())
  );
  System.out.println(chatResponse.getResult().getOutput().getContent());
  return new Quote();
}
```

You should now be able to rebuild and redeploy the app locally, and see a random quote printed to the command line.