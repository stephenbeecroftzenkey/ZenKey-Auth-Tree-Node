![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/ZenKey_rgb.png)
# Integrate the ZenKey Identity Service with a ForgeRock Solution

## ZenKey Identity Service Overview

ZenKey Identity Service is a unique, network-based identity solution that relies on data derived from wireless carriers to verify users. It provides a highly secure way for online services to verify their customers’ identities when they login from mobile devices and, lets people easily log into websites. This solution helps eliminate the need to remember, manage or update dozens of usernames and passwords that customers use today to log into web sites.

The ZenKey Identity Service integration with ForgeRock supports primary device flows, and secondary device flows using a browser on a laptop, desktop, or mobile device. For more information about these authentication flows, see the  <a href="https://developer.myzenkey.com/web" target="_blank">Server and Web Integration Guide</a>.


For information about how to integrate ZenKey into iOS and Android applications, visit the <a href="http://developer.myzenkey.com" target="_blank">ZenKey Developer Resource Site</a>.

## Prerequisites for integrating ZenKey and ForgeRock

 - You must have an instance of ForgeRock Access Management 6.5.0 or greater installed and running.
 - You must be registered in the <a href="https://portal.myzenkey.com/login" target="_blank">ZenKey Developer Portal</a>. Once your company is approved, make note of your client id and client secret. For more information, see the  <a href="https://developer.myzenkey.com/portal/" target="_blank">ZenKey Portal User Guide</a>.
 - The wireless carriers must provision your client. Currently, the provisioning step may take a few days, but will be faster in the future.

## Install the ZenKey Authentication Node

1. In a browser, go to <a href="https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/releases" target="_blank">https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/releases</a>.
2. Download the latest ZenKeyNode .jar file.
3. Install the .jar file on the web server that is hosting Access Management. On Tomcat, put the jar file in the lib directory.
For example: /tomcat/webapps/openam/WEB-INF/lib
4. Restart the server.

## Configure the ZenKey Authentication Node

1. In a browser, login to ForgeRock Access Management with your amadmin credentials `https://am_url.com/openam/console`

4. On the **Realms** page, click **Top Level Realm**.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/TopLevelRealm.png).
 
5. On the **Realm Overview** page, click **Authentication** > **Trees**.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/Trees.png).
 
6. Click **Create Tree**.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/CreateTree.png).
 
7. In the **Tree Name** field, enter a name for the node.

8. Click **Create**.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/TreeName_Create.png).
 
9. In the left side panel under **Components**, type 'ZenKey' in the filter field.  
![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/TypeZenKey.png).

10. When the ZenKey Auth Node appears in the left side panel, select the node and drag it to the main body of the page.

11. Connect the the Start node to the ZenKey node: Drag the green dot on the Start node to the ZenKey Auth node.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/ConnectStartToZKAuthNode.png).
 
12. In the left side panel under **Components**, type 'Provision Dynamic Account' in the filter field.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/TypeProvision.png).
 
13. When the 'Provision Dynamic Account' node appears in the left side panel, select the node and drag it to the main body of the page.

14. Connect the 'No Account exists' dot on the ZenKey Auth Node to the left side dot on the 'Provision Dynamic Account' node.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/ConnectNoAccountToProvision.png).
 
15. In the left side panel under **Components**, type 'Success' in the filter field.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/TypeSuccess.png).
 
16. When the 'Success' node appears in the left side panel, select the node and drag it to the main body of the page.

17. Connect the 'Account exists' dot on the ZenKey Auth Node to the left side dot on the 'Success' node.  

 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/ConnectAccountExistsToSuccess.png).
 
18. Connect the right side dot on the 'Provision Dynamic Account' node to the left side dot on the 'Success' node.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/ConnectProvisionToSuccess.png).
 
19. Delete the 'Failure' node.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/DeleteFailure.png).
 
20. Click **Save**.  
 ![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/ClickSave.png).


## Add Developer Portal credentials to the ZenKey Auth Node

1. Select the ZenKey Auth Node.
2. On the right side of the page, scroll down until you see the **Client ID** field. 
3. Enter your client ID and client secret, from the ZenKey Developer Portal dashboard, into the  **Client ID** and the **Client Secret** fields. Afterwards, the page auto-populates the **Carrier Discovery URL**, **OIDC Provider Config URL**, and the **Redirect URL** fields. You configure the Redirect URL in the Developer Portal.
4. Add scopes for app and site users: On the right side of the page, in the **OAuth** field, enter one or more of the following scopes:
    - openid
    - profile name
    - email
    - phone
5. Click **Save**.

## Configure Account and Attribute mappings

1. Select the ZenKey Auth Node.
2. On the right side of the page, scroll down to the **Account Mapper Configuration** section.
3. Verify the value in the **KEY** field is 'email'.
4. Verify the value in the **VALUE** field is 'uid'.
5. Scroll down to the **Attribute Mapper Configuration** section.
6. Verify the value in the **KEY** field is 'email'.
7. Verify the value in the **VALUE** field is 'uid'.
8. Click **Save**.

## Set the Default Authentication Service to ZenKey

Set ZenKey as the authentication service for all websites that connect to the Access Management server.

1. Click **Top Level Realm**.
2. On the **Realms** page, navigate to **Realm Overview** > **Authentication** > **Settings**.
3. In the **Organization Authentication Configuration** drop down field, select 'ZenKey'.
4. Click **Save Changes**.
5. Logout.

## Test the ZenKey ForgeRock Configuration

You should verify that you correctly integrated ZenKey with your ForgeRock instance.

Steps for testing the ZenKey and ForgeRock configuration
1. Logout of the Access Management instance.

2. Access the Access Management instance again by visiting `https://am_url.com/openam/console`.

3. Log in with ZenKey by selecting the green micro button.
![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/01_sign_in.png)

4. Select your ZenKey account
![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/02_trusted_browser.png)

**Note:** If you are not using a trusted browser, follow the on screen prompts to connect your browser to ZenKey
![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/02_connect_trusted_browser.png)

5. The authorization request is sent to your ZenKey app
![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/03_auth_request_sent.png)

6. Open the ZenKey app on your phone and press continue <br>
![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/continue_all_carriers1.png)<br>  

7. You have successfully logged in within your browser
![](https://github.com/ForgeRock/ZenKey-Auth-Tree-Node/blob/master/Images/06_logged_in.png)
