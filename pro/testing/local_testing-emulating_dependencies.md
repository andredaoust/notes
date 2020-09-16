# Local Testing - Emulating Dependencies
- [Local Testing - Emulating Dependencies](#local-testing---emulating-dependencies)
  - [Running Redis locally with Docker](#running-redis-locally-with-docker)
    - [Additional Notes](#additional-notes)
  - [Running MongoDB locally with Docker](#running-mongodb-locally-with-docker)
    - [Additional Notes](#additional-notes-1)
  - [Service Bus](#service-bus)
    - [Additional Notes](#additional-notes-2)


## Running Redis locally with Docker

1. Have docker setup and running
2. From a docker compatible command line run:

```
docker pull redis
```

3. From the same command line run:
```
docker run -p 6379:6379 --name=redis redis:latest
```

4. Edit your `.env` file's _Redis__ConnectionString=_ value with:

```
localhost:6379,ssl=false,abortConnect=false
```

### Additional Notes  
* A useful tool to view/edit the content of a Redis instance is [Redis Desktop Manager](https://redisdesktop.com/).

---

## Running MongoDB locally with Docker 

1. Have docker setup and running
2. From a docker compatible command line run:

```
docker pull mongo
```

3. From the same command line run:
```
docker run -p 27017:27017 --name=mongo mongo:latest
```
4. Edit your `.env` file's _MongoSettings__ConnectionString=_ to :
```
mongodb://localhost:27017/test?retryWrites=true&w=majority
```
### Additional Notes  
* A useful GUI tool to manage MongoDB is [Robot3t](https://robomongo.org/download)

---


## Service Bus

Currently there is no Docker image or utility to test locally so we need to create a free Service Bus in Azure.

1. Have an Azure account created and logged into the [Azure Portal](https://portal.azure.com)

2. Search for *Service Bus* and select it from the menu.

3. Click **'+ Add'**

4. Choose a **subscription* ('Visual Studio Professional Subscription' is my default)
5. Choose a **Resource group** (or create a new one)
6. Enter a unique **Namespace name**
7. Leave **Location** as **Canada Central**
8. Choose the _Basic_ **Pricing tier**

9. Click **'Review + create'**
0. Click **Create**

1. Wait for the deployment to complete

2. open your new Service Bus

3. Click **'+ Queue'**
4. Enter a unique name in the **Name** field
5. Click **Create**

6. Under the Settings heading on the left side menu, click **'Shared access Policies'**
7. Click the default policy, **RootManageSharedAccessKey** (give it a minute to load)
8. Copy the **Primary Connection String** in the right side menu
9. Edit your `.env` file's _ServiceBusSettings__ConnectionString=_ value to the Primary Connection String.

### Additional Notes  
* A useful tool to browse the Service Bus Queue is [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer)


