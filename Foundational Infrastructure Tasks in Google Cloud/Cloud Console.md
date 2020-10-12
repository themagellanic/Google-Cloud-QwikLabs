## Cloud Storage: Qwik Start - Cloud Console

**The very first step is as usual log in to Google console with the lab credentials**

# Task1
## Create a bucket

In the Cloud Console, go to Navigation menu > Storage > Browser. Click Create Bucket:
![Something](https://cdn.qwiklabs.com/K3M3mS%2F5XbEv%2BtqWj7%2FyU7B98yazE3fgw%2FHwBbznwzo%3D)

**Name(Read the rules)**: Enter a unique name for your bucket.

**Storage class**: Multi-Regional

**Location**: United States

### Like this 

![something2](https://cdn.qwiklabs.com/zu%2BzJrePKI8%2BzSw%2FJh5fEWrmiY8ZVgni4qYvYOGnbUw%3D)

Q1.*Every bucket must have a unique name across the entire Cloud Storage namespace.*

Solution:True

*Q2. Cloud Storage offers which storage classes:*

Solution:Google Cloud Storage offers four storage classes with progressively lower storage costs, but higher retrieval costs: Standard (frequently used data),
Coldline Storage (rarely used data), Nearline Storage (infrequently used data), and Archive Storage (long term archival).
# Task2
## Upload an Object into the Bucket
You can upload using both options drag & drop or using upload  button
![Image](https://cdn.qwiklabs.com/n1VFky%2BpITzj%2FVxVE3llbhvsvyv4sGTg2tuZZxo7ZaA%3D)
# Task3
## Share an Object Publicly

To create a publicly accessible URL for the object, click the drop-down menu (three vertical dots).

Select Edit permissions from the drop-down menu.

In the dialog that appears, click the + Add entry button.

Add a permission for all users by entering in the following:

1.Select Public for the Entity.
2.Enter allUsers for the Name.
3.Select Reader for the Access.
Then click Save:

# Task4
## Create Folders

In this section you will create a folder.

1.Click the Create Folder link near the top of the page.

2.Name the folder folder1, then click Create.

You should see the folder in the bucket with a folder icon to distinguish it from other objects:

