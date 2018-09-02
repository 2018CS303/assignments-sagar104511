## Assingment_2_1

### Installing Blue Ocean
- From an administrator account, go to `Manage jenkins -> Manage plugins`.
- Install the `Blue Ocean (BlueOcean Aggregator)` plugin from the `Available` plugins. This is available after jenkins 2.7.
- This will install Blue Ocean and the required dependencies.
- Restart jenkins by entering command `sudo systemctl restart jenkins` in the terminal.
- Upon restart, the menu will now have an option `Open Blue Ocean`, click on it to start using Blue Ocean GUI.

### Building a private Github Repository as a FreeStyle project
- Create a new freestyle project
- Select `Git` in the section `Source Code Management`
- In the input field `Repository URL` enter the URL of your private repository.
- Since the repository is private, jenkins will not be able to access it without credentials. Click on `Add` in the `Credentials` input field and enter your github credentials.
- Once added, select the credentials from the dropdown menu for `Credentials`.
- You can select the branches to build or leave it as `*/master` (default).
- Now the private repository can be used like any other public repository.

### Using Git SCM Poll
- You will need to map your `localhost:8080` (where jenkins is running) to a public IP for this.
- Go to `Build Triggers` section and choose `GitHub hook trigger for GITScm polling`.
- #### Getting a Public IP (using `ngrok`)
  - You can get the public IP of your `localhost:8080` (where jenkins is running) using `ngrok`.
  - Go to [ngrok's official site](https://ngrok.com/download) and follow the instructions to install ngrok. 
  - Run `./ngrok http 8080` (to map 8080 which is the jenkins port to a public IP). You will get a public IP of the form `http://<SOMETHING>.ngrok.io`, use this in the webhook.
- #### Setting up webhook in the Github repository.
  - Navigate to `Settings` in your Github repository.
  - Go to `Webhooks -> Add webhook`.
  - Enter `http://<public-ip or URL>/github-webhook/` in `Payload URL`.
  - Select `Content type` as `application/json`.
  - You can select `Which events would you like to trigger this webhook?` as per requirements.
  - The `Active` option should be selected.
  
- Now Github will make a request to the Jenkins webhook and cause a build whenever required.

### Post-build Actions - Archive the artifacts
Here, we adding the `Archive` post build action. This will archive selected files.

- Select the project and go to `Configure > Post-build Actions`
- Click the `Add post-build action` button and select `Archive the artifacts`
- Select the files that you want to archive (eg: `*` will archive all the files)
- Click the `Advanced` button
- Select
	- `Use default excludes`
	- `Archive artifacts only if build is successful`
- This will archive all files in the linked github repo when the build is successful
