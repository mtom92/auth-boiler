# Node/Express/PostgreSQL Boilerplate

This is a bare-bones Node/Express app with basic user authentication and authorization . It
has local auth on the `master` branch and additionally Facebook auth on the `with-Facebook` branch.
This boilerplate exist so I don't need to create a new project from scratch every time I need
a project with working auth.

## What it includes

* Sequelize is set up for PostgreSQL
* Sequelize model and migration(s) for users
* Passport , Express-Session , and Connect-Flash modules
* Error/Succes message alerts
* BCrypt for hashing password

### User Schema

| Column | Data Type | Description |
| ------------ | --------- | ----------------------------------------------|
| id | Integer | Primary Key |
| firstname | String | - |
| lastname | String | - |
| email | String | usernameField for login |
| password | String | hashed with BCrypt before creation of new row |
| birthday | Date | Might want to use moment module to format this |
| admin | Boolean | Set default value to false |
| image | Text | A URL to an image of the user - required field |
| bio | Text | - |

Additional fields from `with-facebook`

Column | Data Type | Description |
| ------------ | --------- | ----------------------------------------------|
| facebookId | String | Facebook Profile Id |
| facebookToken | String | Facebook login token |


This is the default schema provided . Add additional migrations as needed for more data.


## Routes Table
 By default , the following routes are provided

 | Method | Path | Location | Purpose |
 | ------- | -------------- | ----------------- | ------------------- |
 | GET     | /              | index.js        | Home Page |
 | GET     | /profile       | controllers/profile.js | User Profile Page |
 | GET     | /profile/admin | controllers/profile.js | Admin Dashboard Page |
 | GET     | /auth/login    | controllers/auth.js | Renders Login Form |
 | POST    | /auth/login    | controllers/auth.js | Handles Login Auth |
 | GET     | /auth/signup    | controllers/auth.js | Renders Signup Form |
 | POST    | /auth/signup    | controllers/auth.js | Handles Signup Auth |
 | GET     | /auth/logout    | controllers/auth.js | Removes users session |

Additional Routes

| Method | Path | Location | Purpose |
| ------- | -------------- | ----------------- | ------------------- |
| GET     | /auth/facebook | controllers/auth.js | Outgoing Request to Facebook |
| GET     | /auth/callback/facebook | controllers/auth.js | Incoming data from Facebook |

## Steps To Use

#### 1. Clone this repository, but with a different Name

On the terminal run :

*Part A: Decide if using facebook*

If you dont need facebook auth , then use the `master branch`

```
git clone <repo_link> <new_name>
```

#### 2. Decide what the new project needs

If you do not need facebook auth, then use the `master` branch. Otherwise, switch to
the `with-facebook` branch with this command :

```
git checkout with-facebook
```

>Note : If using facebook, you will need to set up a new app on developers.facebook.com

* Part B: Remove stuff nor being used

#### 3. Install node modules form package.json

On the terminal run:

```
npm install
```

> Tip : `npm i` can be used as a shortcut

#### 4. Restructure Git Remotes

Basically, this is git's version of updating the address book.

* First, remove the "old" remote.
  * `git remote remove origin `
* Then go to github and create a new , empty repository
* Copy the new repository repository link
* Set up a new remote pointing to the new repository
 * `git remote add origin <new_repo_link>`

#### 5. Make a new .env file

At a minimum the following is needed :

```
SESSION_SECRET = 'This is a string for the session to use (like salt)'
```
Optional others, including facebook specific ones:

```
SESSION_SECRET = 'My secret pass phrase'
FB_APP_ID = '816684688731094'
FB_APP_SECRET = 'd6e31f3e770dd34417e978e3d569cf91'
BASE_URL = 'http://localhost:3000'

```

#### 6. Customize with the new project's name

* Title in `layout.ejs`
* Logo and links in `nav.ejs`
* The name , description , and repo fields in `package.json`
* Remove the `read.md` content (this) and put a stub for the new project's readme

#### 7. Create a new database for the new project

#### 8. Set up sequelize

First . update the development settings in the `config/config.json`

(Optional) If additional fields on the user table are needed, follow directions [here] (#adding-migrations)
to create additional migrations.

Then , do the Sequelize migrations with this command :

```
sequelize db:migrate

```

#### 9. Run the server locally and ensure that it works

If you have `nodemon`installed globally , run `nodemon`as a command in the root folder.

Otherwise , run `node index.js`.

Unless sepecified otherwise, the port in use will be 3000.

#### 10. Commit and push to your new project

```

git add -A
git commit -m "Initial commit"
git push origin master

```

> Note : We switched the origin remote to this point to the new github project on step 4 . Make sure that
this is done properly by running the command `git remote -v` to check the remote locations

#### 11. Next Steps

Assuming that the set up steps went smoothly , now you can add new models/migrations, new
controllers and routes, etc., and just in generally start developing as if you had started from
scratch.

## Notes on Optional Steps

#### Adding migrations

* STEP 1:Create a migration file via sequelize command line
 * `sequelize migration:create --name add-age`
* STEP 2:Write the up and down functions of the migration
 * Refer to other migrations for how this looks
 * The part to add looks like : `return queryInterface.addColumn('users','age',
  sequelize.INTEGER)`
* STEP 3: Add the column into the user model
 * `user.js` - located in the models folder

### Facebook App Set Up

>Note : A Facebook login is required

* Go to developers.facebook.com
   * Create a new App
* Add a platform : website
* Add a product : Facebook login
   * Set a valid OAuth  Redirect URL to https://yoursite.com/auth/callback/facebookId
* Copy the App Id and App Secret to the `.env` file 
