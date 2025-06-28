8. Create a playbook called test.yml as per the following details:

    a) The playbook runs on managed nodes in the test host group.
    b) Create the directory /webtest with the group ownership webtest group and having the regular permissions rwx for the owner and group and rx for the others.
    c) Apply the special permissions: set group ID
    d) Symbolically link /var/www/html/webtest to /webtest directory.
    e) Create the file /webtest/index.html with a single line of text that reads: Testing.
