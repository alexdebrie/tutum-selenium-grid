# Running Selenium Grid on Tutum  

[![Deploy to Tutum](https://s.tutum.co/deploy-to-tutum.svg)](https://dashboard.tutum.co/stack/deploy/)

Quickly launch a Selenium Grid cluster on Tutum to test your application code
in parallel in multiple browsers.

### What is Selenium?

[*Selenium automates browsers*](http://www.seleniumhq.org/). With Selenium, you
can easily test your code to make sure you have consistent behavior, from Firefox
to Chrome to Internet Explorer. Selenium is also great as part of an integration
test suite to ensure your components are working together.

### What is Selenium Grid?

[Selenium Grid](http://docs.seleniumhq.org/docs/07_selenium_grid.jsp) allows for
parallel testing with Selenium. You have a central "Hub" which receives
requests for tests and routes them to a registered "Node". A Node is configured
with certain properties, such as its operating system and browser. When the Hub
receives a request for a test with a Linux operating system and a Firefox browser,
it will make sure the test is run by the proper Node. This allows for faster, more
powerful testing with Selenium.

# How to get started

### Deploy to Tutum

[Sign up](https://dashboard.tutum.co/accounts/register/) for a Tutum account and
deploy your first Node Cluster with a tag of `selenium`. When deploying, I
recommend using a Node with at least 1 GB of RAM to handle the Hub and Nodes.

Once your Node Cluster is deployed, hit the **Deploy to Tutum** button at the
top of this Readme. Congratulations! You've deployed a Selenium Grid cluster 
with two Firefox Nodes and two Chrome Nodes!

### View your Hub configuration

Once your Grid is launched, you can check the config at the endpoint of your
`hub` service. To see this, use your browser to visit the endpoint of your
`hub` with a path of `/grid/console/`. 

For example, if my endpoint is `hub.selenium.username.svc.tutum.io:4444/`, I would
point my browser to `http://hub.selenium.username.svc.tutum.io:4444/grid/console`
You should see four browsers listed, two each of Firefox and Chrome.

### Run your test!

You'll need to check the documentation for your preferred language. I use Python,
and the Selenium documentation for the Python bindings can be found [here](https://selenium-python.readthedocs.org/index.html).

You'll want to instantiate a remote WebDriver that points to your Selenium Hub
before calling your tests. Using a modified version of the example in the Python
docs [here](https://selenium-python.readthedocs.org/getting-started.html#using-selenium-with-remote-webdriver),
it would look something like the following:

    from selenium import webdriver
    from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
    
    driver = webdriver.Remote(
        command_executor='http://hub.selenium.username.svc.tutum.io:4444/wd/hub',
        desired_capabilities=DesiredCapabilities.CHROME)
        
**Be sure to substitute your endpoint in the `command_executor` line above!**

# Additional Integrations

### Additional Capabilities

Currently, you cannot create a Windows container, so you are limited to testing on
Linux operating systems if you want to put your Nodes in Docker containers. However,
you can add additional Nodes to your Hub, even if they're not Docker-ized.

Check out [Distributed Testing with Selenium Grid](https://www.packtpub.com/sites/default/files/downloads/Distributed_Testing_with_Selenium_Grid.pdf)
to see how to set up Windows and Mac Nodes. You can even run iWebDriver on an iOS
device to test application behavior natively on your iPad or iPhone!

### Debug Mode

This repo uses images from the [Docker selenium](https://github.com/SeleniumHQ/docker-selenium)
GitHub repo. This repo also includes "Debug" Nodes, which allow you to use a VNC
client to view your tests running. This can help you understand why your tests
are failing. 
