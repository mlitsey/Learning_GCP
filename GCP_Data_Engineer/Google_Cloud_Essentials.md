



# Deploying an App Engine Application

## Introduction

In this hands-on lab, we’ll deploy an app to App Engine that allows users to enter details into a NoSQL database, Cloud Datastore, and displays the proper HTML template and CSS. The process requires that we customize the `config.py` file before deploying the app.

## Deploying an App Engine Application

Now, on to the lab!

### Enable APIs and clone GitHub repository.

1. Navigate to **APIs & Services** > **Library**, and search for "Datastore".
    
2. Select the **Cloud Datastore API** card.
    
3. Click **Enable**.
    
4. Head back to **APIs & Services** > **Library**.
    
5. Search for "Storage".
    
6. Select the **Cloud Storage** card.
    
7. We should see it's already enabled. (If it isn't, click **Enable**.)
    
8. Repeat the process to enable both the **Cloud Build API** and **Container Registry API**
    
9. Click the icon in the upper right navigation menu to **Activate Cloud Shell**.
    
10. From the Cloud Shell, create the necessary bucket:
    
    `gsutil mb -c regional -l us-east1 gs://<BUCKET NAME>`
    
11. In order to make the objects publicly viewable, we need to add a couple roles:
    
    `gsutil iam ch allUsers:objectViewer gs://<BUCKET NAME>`
    
12. Clone the GitHub repo to bring in all the files we need:
    
    `git clone https://github.com/linuxacademy/content-gc-essentials`
    
13. Change directory to the `content-gc-essentials/app-engine-lab` folder:
    
    `cd content-gc-essentials/app-engine-lab`
    

### Configure `config.py` file.

1. Click the pencil icon to launch the Cloud Shell Editor.
2. From the Cloud Shell Editor, open `config.py` in the `app-engine-lab` folder.
3. Change `PROJECT_ID` to the current project, as shown in the Cloud Shell.
4. Change `CLOUD_STORAGE_BUCKET` to your bucket name.
5. Click **File** > **Save**.

### Deploy the app.

1. In the Cloud Shell, enter the following code:
    
    `gcloud config set app/cloud_build_timeout 6000s gcloud app deploy`
    
2. When prompted, choose the us-east1 region.
    

### Test the app.

1. In the Cloud Shell, enter the following code:
    
    `gcloud app browse`
    
2. If the browser window does not open, click the generated link.

![](2022-07-27-09-57-54.png)




# Working with Compute Engine Windows Instances

In this hands-on lab, we’ll experience the creation of a Windows-based Compute Engine VM instance, set up an IIS server, and push our first web page live to confirm the server’s operation.

## Working with Compute Engine Windows Instances

Now, on to the lab!

### Create a Compute Engine VM instance.

1. From the main navigation, choose **Compute Engine** > **VMs**.
2. Click **Create**.
3. With _New VM instance_ chosen from the options on the left, configure your instance:
    - _Name_: Provide a relevant name using hyphens, like "la-windows-1".
    - _Region_ Leave as-is
    - _Zone_: Leave as-is
    - Series: N1
    - _Machine type_: n1-standard-1
    - _Boot disk_:
        - Click **Change**.
        - Select **Windows Server 2019 Datacenter, Server with Desktop Experience**.
        - Click **Select**.
    - _Identity and API access_: Leave as-is
    - _Firewall_: **Allow HTTP traffic**
4. Click **Create**.

### Set Windows password.

1. Once the instance is created, click the arrow next to **RDP** in the _Connect_ column, and select **Set Windows password** from the dropdown.
2. In the dialog, confirm your username is correct, and click **Set**.
3. Copy the supplied password, and click **Close**.

### Launch RDP window.

1. Launch the RDP window by using one of the following methods:
    - If you're on a Windows system, click **RDP**.
    - If you're on a Mac using Chrome in a standard window, first install the Chrome RDP extension, and then click **RDP**.
    - If you're on a Mac using another browser or incognito window, from the App Store, download and install the latest version of the Microsoft Remote Desktop app. Then, choose **Download the RDP file** from the RDP options and open the file.

### Install IIS.

1. From the Windows Start menu, right-click on Windows Powershell, choose **More** and then **Run as administrator**.
    
2. In the PowerShell window, enter the following commands to set install IIS, starting with:
    
    `import-module servermanager`
    
3. Set up the web server:
    
    `add-windowsfeature web-server -includeallsubfeature`
    
4. Create the `index.html` page:
    
    `echo '<!doctype html><html><body><h1>Greetings from Linux Academy!</h1></body></html>' > C:\inetpub\wwwroot\index.html`
    

### Test your page.

1. From the Compute Engine _VM instances_ page, click the external IP link for the Windows VM instance.
2. Review the page in the browser.

![](2022-07-27-09-39-06.png)




# Triggering a Cloud Function with Cloud Pub/Sub

Cloud Functions can be triggered in 2 ways: through a direct HTTP request or through a background event. One of the most frequently used services for background events is Cloud Pub/Sub, Google Cloud’s platform-wide messaging service.

In this pairing, Cloud Functions becomes a direct subscriber to a specific Cloud Pub/Sub topic, which allows code to be run whenever a message is received on a specific topic. In this hands-on lab, we’ll walk through the entire experience, from setup to confirmation.

## Solution

On the lab page, right-click **Open GCP Console** and select the option to open it in a new private browser window (this option will read differently depending on your browser — e.g., in Chrome, it says "Open Link in Incognito Window"). Then, sign in to Google Cloud Platform using the credentials provided on the lab page.

On the _Welcome to your new account_ screen, review the text, and click **Accept**. In the "Welcome L.A.!" pop-up once you're signed in, check to agree to the terms of service, choose your country of residence, and click **Agree and Continue**.

### Enable Required APIs

1. From the main console navigation, go to **APIs & Services** > **Library**.
2. Search for **Pub/Sub**.
3. Select the **Cloud Pub/Sub API** card.
4. Click **ENABLE**, if displayed.
5. Return to the API Library and search for **functions**.
6. Select the **Cloud Functions API** and click **ENABLE**.
7. Return to the API Library and search for **build**.
8. Select the **Cloud Build API** and click **ENABLE**.

### Create Pub/Sub Topic

1. Using the main navigation menu, under _Analytics_ go to **Pub/Sub** > **Topics**.
2. Click **CREATE TOPIC**.
3. For the _Topic ID_, enter a name for the topic (e.g., "greetings").
4. Click **CREATE TOPIC**.

### Create a Cloud Function

1. Using the main navigation, under _SERVERLESS_, go to **Cloud Functions**.
2. Click **CREATE FUNCTION**.
3. Configure the function with the following values:
    - _Name:_ **acg-pubsub-function**
    - _Region:_ **us-central1**
    - _Trigger:_ **Cloud Pub/Sub**
    - _Topic:_ The topic you just created
4. Click **SAVE**.
5. Click **NEXT**.
6. For the _Runtime_, select **Python 3.9**.
7. Using the _Inline Editor_, select the `main.py` file.
8. Delete the existing code, and paste in the following:

`import base64 def greetings_pubsub(data, context): if 'data' in data: name = base64.b64decode(data['data']).decode('utf-8') else: name = 'folks' print('Greetings {} from Linux Academy!'.format(name))`

1. Set _Entry point_ to **greetings\_pubsub**.
2. Click **DEPLOY**.
    
    > _Note:_ This process can take up to two minutes to complete.
    

### Publish Message to Topic From Console

1. Click the newly created Cloud Function.
2. Switch to the **Trigger** tab.
3. Click the topic link to go to the Cloud Pub/Sub topic.
4. Scroll down to the bottom of the page and switch to the **MESSAGES** tab.
5. Click **PUBLISH MESSAGE**.
6. Under _Message body_, enter the following "everyone around the world".
7. Click **PUBLISH**.

### Confirm Cloud Function Execution

1. Return to the Cloud Functions dashboard and click the **LOGS** tab.
2. Review the logs and confirm the function was executed successfully.

### Trigger Cloud Function Directly From Command Line

1. At the top of the page, click the _Project_ field, and copy the project `ID`.
    
2. Click the icon in the top right corner of the console to activate Cloud Shell.
    
3. When prompted, click **Continue**.
    
4. Enter the following to set the project ID:
    
    `gcloud config set project <PROJECT_ID>`
    
5. When prompted, click **Authorize**.
    
6. Using the following, set a variable called `DATA`:
    
    `DATA=$(printf 'my friends' | base64)`
    
7. Using the `DATA` variable, trigger the function:
    
    `gcloud functions call acg-pubsub-function --data '{"data":"'$DATA'"}'`
    
8. Review the logs and confirm the function was executed successfully.
    

### Publish Message to Topic From Command Line

1. In the Cloud Shell, enter the following command:
    
    `gcloud pubsub topics publish greetings --message "y'all"`
    
2. Review the logs and confirm the function was executed successfully.

![](2022-07-27-10-00-03.png)




# Setting Cloud Storage Lifecycle Rules

While saving an object in a Cloud Storage bucket is relatively inexpensive, there is, nonetheless, a cost. The cost varies depending on the storage class selected. Certain objects are required to be more available at first, requiring the storage class with the highest availability — and cost. Such objects may eventually be relegated to less available and less expense storage classes and even be deleted. Management of these objects over time can be handled automatically by establishing and implementing lifecycle rules. In this hands-on lab, you’ll see how to set a variety of lifecycle rules for Google Cloud Storage buckets, both from the console and the command line, as well as see how to place an object on hold so it stays in place regardless of the rules.

## Solution

### Create and Populate the Storage Bucket

#### Create the Bucket

1. From the Google Cloud console main navigation (hamburger menu on the top left), select **Cloud Storage**.
2. Click **CREATE BUCKET**.
3. Enter a globally unique name for your bucket (e.g., `acg-lifecycle-` with some numbers appended to the end), and click **CONTINUE**. (Remember this name for use later in the lab.)
4. For **Location Type**, select **Region**, use the default region (i.e., **us-east1**), and click **CONTINUE**.
5. For the **default storage class**, select **Standard** and click **CONTINUE**.
6. Click **CREATE**.

#### Add the Files

1. Click the icon for the Cloud Shell (on the top right of the screen, next to the search box).
    
2. Click **CONTINUE**.
    
3. Click the project name on the top left of the screen (next to the Google Cloud logo), and in the **Select a project** pop-up, copy the **ID**.
    
4. Close the pop-up, and set the project within the Cloud Shell. Replace `<PROJECT_ID>` with the ID you just copied:
    
    `gcloud config set project <PROJECT_ID>`
    
5. When prompted, click **AUTHORIZE**.
    
6. Clone the course repo:
    
    `git clone https://github.com/linuxacademy/content-gc-essentials`
    
7. Change to the lab's directory:
    
    `cd content-gc-essentials/cloud-storage-lifecycle-lab`
    
8. Review the files within the directory:
    
    `ls`
    
    Observe the directory includes a few `.jpeg` files as well as a `.json` file, which you'll use for setting some rules.
    
9. Copy the files to the storage bucket. Replace `<BUCKET_NAME>` with the name you created before:
    
    `gsutil cp * gs://<BUCKET_NAME>`
    
10. On the top right of the main console (still on the **Bucket details** page), click **REFRESH** to see the files now available within the bucket.
    

### Create Lifecycle Storage Rules via the Console

Using the console, create the following rules.

#### Nearline Rule

1. From the **Bucket details** page, select the **LIFECYCLE** tab.
2. Click **ADD A RULE**.
3. Under **Select an action**, ensure **Set storage class to Nearline** is selected.
4. Click **CONTINUE**.
5. Under **Select object conditions**, select **Age** and enter _30_ in the text box that is displayed.
6. Click **CONTINUE**.
7. Click **CREATE**.

#### Coldline Rule

1. Click **ADD A RULE**.
2. Under **Select an action**, select **Set storage class to Coldline**.
3. Click **CONTINUE**.
4. Under **Select object conditions**, select **Age** and enter _90_ in the text box that is displayed.
5. Click **CREATE**.

#### Archive Rule

1. Click **ADD A RULE**.
2. Under **Select an action**, select **Set storage class to Archive**.
3. Click **CONTINUE**.
4. Under **Select object conditions**, select **Age** and enter _365_ in the text box that is displayed.
5. Click **CREATE**.

### Update Lifecycle Rules via the CLI

1. For the final rule, go back to the Cloud Shell terminal.
    
2. Click **Open Editor** (above the code entry area) to launch the Cloud Shell Editor.
    
    > **Note**: Depending on your browser, a message may be displayed indicating that you need to open the Editor in a new window. In this case, click **Open in a new window**.
    
    > **Troubleshooting Tip**: If you're using Google Chrome and receive an error message stating **We can't load the code editor here because third-party cookies are disabled**, follow these steps to enable third-party cookies:
    > 
    > 1. In the address bar, click the eye icon.
    > 2. Click **Site not working?**
    > 3. Click **Allow cookies**, and reload the page.
    > 4. Click **Open Editor** again.
    
3. From the file navigation on the left, navigate through the directory structure to `content-gc-essentials` > `cloud-storage-lifecycle-lab`, and select the `delete-after-two-years.json` file to review it. Observe the final rule that will be added. The actions within the file will override the existing lifecycle rules.
    
4. If you opened a new window, go back to the previous Cloud Shell window, and click **Open Terminal** on the bottom. If you opened Cloud Editor within the same window, click **Open Terminal**, and drag the terminal view down so you can also see the main console.
    
5. Set the lifecycle rules using the JSON file (replace `<BUCKET_NAME>` with the bucket your previously created):
    
    `gsutil lifecycle set delete-after-two-years.json gs://<BUCKET_NAME>`
    
6. Back in the console (still within the **LIFECYCLE** tab of the **Bucket details** page), click **REFRESH**. Confirm the new **Delete object** lifecycle rule has been added in the console.
    

### Place an Item on Hold

1. Select the **OBJECTS** tab.
2. Select the checkbox for the `hold-this-image.jpeg` object.
3. Above the file list, click **MANAGE HOLDS**.
4. Select **Event-based hold**.
5. Click **SAVE HOLD SETTINGS**. Observe the object now has a different **Retention expiration date** of **Determined upon hold removal**.




