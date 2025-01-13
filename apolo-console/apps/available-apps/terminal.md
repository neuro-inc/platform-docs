# Terminal

The Apolo Terminal app allows users to execute shell commands, manage files, and interact with computational resources directly through a web-based interface. This guide provides step-by-step instructions for accessing, installing, and using the Terminal app.

## Accessing the Terminal App

Navigate to the **Apolo Console** and click on the **Apps** section from the left-hand navigation menu. You will see a list of available apps, as shown in the screenshot below.  

![All Apps View](<../../../.gitbook/assets/console_screenshots/Terminal_app_1.png>)  

Locate the **Terminal** app. If it is not yet installed, proceed to the installation steps.  

## Installing the Terminal App 

1. Select the Terminal App:  
   Click on the button "Install" located on the **Terminal** app card.  
   
2. **Configure Resources**:  
   After selecting the Terminal app, you will be directed to the installation page. Under the **Resources** tab, choose a preset configuration based on your computational needs (e.g., `cpu-small`, `cpu-medium`).  
   
   You can also add environment variables or secrets as needed. Use the **Add secret** or **Add variable** buttons to configure these parameters.  

   ![Resources Configuration](https://example-link.com/screenshot2)  

3. **Add Metadata**:  
   In the next step, under the **Metadata** tab, provide a name, description, and any relevant tags for the job.  

   ![Metadata Configuration](https://example-link.com/screenshot3)  

4. **Install the App**:  
   Click on the **Install App** button to complete the installation process. The app will appear in the "Installed Apps" tab once successfully deployed.  

---

## **3. Managing Installed Terminal Apps**  

To view and manage installed instances of the Terminal app:  

1. Go to the **Installed Apps** tab.  
2. You will see a list of running Terminal app instances. Each instance displays:  
   - Preset name  
   - Status (e.g., "Running")  
   - Kind and target details  

   ![Installed Apps](https://example-link.com/screenshot4)  

---

## **4. Using the Terminal App**  

1. **Launch the Terminal**:  
   Click on an instance of the Terminal app from the "Installed Apps" list. This will open a web-based shell environment.  

2. **Run Commands**:  
   Use the terminal to execute shell commands. A welcome message provides helpful examples of commands you can run, such as managing Apolo CLI workflows and running jobs.  

   Example:  
   ```bash
   apolo run alpine:latest echo 'Hello, World!'
   ```  

   ![Terminal Interface](https://example-link.com/screenshot5)  

3. **Kill Running Jobs**:  
   To stop active processes, use the provided commands in the terminal interface, such as:  
   ```bash
   apolo-flow kill jupyter
   apolo kill $(hostname)
   ```  

---

## **5. Additional Resources**  

For detailed usage instructions, refer to the official Apolo documentation or contact support using the **Contact Support** link in the Apolo Console sidebar.  
