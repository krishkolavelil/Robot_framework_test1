# Robot_framework_test1
robot frame work test for Github and jenkins
*** Settings ***
Library           Selenium2Library
Library           Compare_string.py
Library           Collections
Library           OperatingSystem
Library           String

*** Variables ***
${Username}       admin
${Password}       MaharaDemo
${Browser}        Chrome
${SiteUrl}        http://demo.mahara.org/
${startpage}      About this demo site
${DashboardTitle}    Dashboard - Mahara Demo
${Delay}          5s
${Delay1}         15s
${NewGroup}       yash1 technologies
${sucessmessage}    Group saved successfully
${log1}           Group already exists

*** Test Cases ***
LoginTest
    Open Browser to the Login Page
    Enter User Name
    Enter Password
    Click Login
    sleep    ${Delay}
    Assert Dashboard Title
    Create new Group
    sleep    ${Delay1}

Closethetab
    Close Browser
    [Teardown]    Close Browser

*** Keywords ***
Open Browser to the Login Page
    open browser    ${SiteUrl}    ${Browser}
    Maximize Browser Window

Enter User Name
    Input Text    id=login_login_username    ${Username}

Enter Password
    Input Text    id=login_login_password    ${Password}

Click Login
    click button    Login

Assert Dashboard Title
    Title Should Be    ${DashboardTitle}

Check For Corrent Page
    ${header}    Get Text    xpath=.//*[@id='dropdown-nav']/li[4]/a
    Log    ${header}
    Should Be Equal As Strings    ${header}    ${startpage}

Click On Link_1
    Mouse Down On Link    xpath =.//a[@href='http://demo.mahara.org/group/mygroups.php']
    sleep    ${Delay1}
    Click Link    xpath =.//a[@href='http://demo.mahara.org/user/find.php']

Create new Group
    Click Link    xpath=.//a[@href='http://demo.mahara.org/group/mygroups.php']
    Click Link    xpath=.//a[@href='http://demo.mahara.org/group/edit.php']
    Input Text    id=editgroup_name    ${NewGroup}
    Click Button    id=editgroup_submit
    ${status}=    Get Text    xpath=.//*[@id='messages']/div/div
    ${value}    Compare_strings    ${status}    ${sucessmessage}
    run keyword if    ${value}==False    Compare_string.Groupalreadyexist
    ...    ELSE    Compare_string.Groupcreatedsucessfull    ${NewGroup}
