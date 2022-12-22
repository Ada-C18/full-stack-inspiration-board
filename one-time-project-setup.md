# One-time Project Setup

## Logistics

1. Pick one group member to [fork the front-end layer repo](https://github.com/Ada-C18/front-end-inspiration-board), and add everyone as collaborators.
1. Pick either the same or a different group member to [fork the back-end layer repo](https://github.com/Ada-C18/back-end-inspiration-board), and add everyone as collaborators.
1. Designate one pair to go through the front-end setup steps, and another pair to go through the back-end setup steps.

Splitting this work allows the pairs to commit and push the initial setup. Then, the rest of the group can pull down that work, which will help everyone!

# Front-End Layer Setup

Feel free to follow these steps on one machine in this order exactly. Feel free to ask questions and get help from your teammates when you need!

<details>

<summary>Click here to expand front-end layer setup steps.</summary>

## Clone

Clone the forked repo. Do _not_ clone this inside of another project folder, because that will cause issues.

## Scaffold the App

Create a new React app within this project folder. **You must perform this within this front-end project folder**.

```bash
$ npx create-react-app .
```

## Add `axios`

Install axios:

```bash
$ yarn add axios
```

## Creating a `.env` File

Create a file named `.env`.

The front-end layer needs to send API requests to the back-end layer. In order to handle this, the front-end layer repo **must** include a `.env` file with this line:

```
REACT_APP_BACKEND_URL=http://localhost:5000
```

Note that this `REACT_APP_BACKEND_URL` _must_ include `http://`.

Use this environment variable to send your API requests. You can read it by using the expression `process.env.REACT_APP_BACKEND_URL`. For example, we may use it like this in any component:

```js
axios.get(`${process.env.REACT_APP_BACKEND_URL}/boards`, {
    // ...
```

This will make Heroku deployment easier.

## Commit and Push

Commit and push your files to your repo, especially including the `package.json` file!

</details>

# Back-End Layer Setup

Feel free to follow these steps on one machine in this order exactly. Feel free to ask questions and get help from your teammates when you need!

Getting one or two members to do the back-end layer setup first is helpful. This pair can create the models, which will create the migration files. When other members can pull the migration files from git and run the migrations, it's a lot smoother!

<details>

<summary>Click here to expand back-end layer setup steps.</summary>

## Clone

Clone the forked repo. Do _not_ clone this inside of another project folder, because that will cause issues.

## Managing Dependencies

Create a virtual environment:

```bash
$ python3 -m venv venv
$ source venv/bin/activate
(venv) $ # You're in activated virtual environment!
```

Install dependencies (we've already gathered them all into a `requirements.txt` file):

```bash
(venv) $ pip install -r requirements.txt
```

## Setting Up The Database

Create a database named `inspiration_board_development`.

## Creating a `.env` File

Create a file named `.env`.

Add this environment variable: `FLASK_ENV=development`

Also, add the environment variable `SQLALCHEMY_DATABASE_URI` to hold the path to your development database.

Your `.env` may look like this:

```
FLASK_ENV=development
SQLALCHEMY_DATABASE_URI=postgresql+psycopg2://postgres:postgres@localhost:5432/inspiration_board_development
```

## Create Models

Consider these two initial models with these attributes:

`Board`, **table name: `board`**

- `board_id`, int, primary key
- `title`, string
- `owner`, string

`Card`, **table name: `card`**

- `card_id`, int, primary key
- `message`, string
- `likes_count`, int

Then follow these steps. Recall that we can update our models any time. These steps are to initialize our database:

1. Run `flask db init` (Do this before making the model files.)
1. Create the model files for them
1. Update `app/__init__.py` to import the two models into `create_app`
1. Run `flask db migrate -m "adds Board and Card models"`
1. Run `flask db upgrade`

## Run `$ flask run` or `$ FLASK_ENV=development flask run`

Check that your Flask server can run with `$ flask run`.

The environment variable in the `.env` file, `FLASK_ENV`, will automatically enable development mode. This enables hot-reloading, which is a feature that refreshes the Flask server every time there is a detected change.

Alternatively, if our environment variable `FLASK_ENV` is not enabling development mode, we can manually set it with `$ FLASK_ENV=development flask run`.

**It is highly recommended to run the Flask servers in development mode**.

## Commit and Push

Commit and push your files to your repo, especially including the migration files!

</details>

# Coming Back Together

Get all members aligned on the front-end. Get all members to...

1. Clone the repo and pull changes
1. Create an identical `.env` file on their local machine
1. Install dependencies using `yarn add`

Get all members aligned on the back-end. Get all members to...

1. Clone the repo and pull changes
1. Create the `inspiration_board_development` database on their local machine
1. Create an identical `.env` file on their local machine
1. Create a virtual environment and activate it
1. Install the dependencies from `requirements.txt`
1. Run `flask db upgrade`

## Next Steps: Feature Development

The next step is to pursue feature development!

Read through [the project requirements](./project-requirements.md), and make a strategy. This is the recommended strategy:

1. Some folks pair and begin front-end development.
    - Start with displaying:
        - A list of Boards
        - A form to create a new Board
    - From the requirements, we can infer that each Board has a `title`, `owner` name, and `board_id` (as the hidden, implied primary key).

1. Other folks pair and begin back-end development.
    - Start with making the conventional endpoints for:
        - Getting a list of all boards
        - Creating a board
    - From the requirements, we can infer that the front-end layer is expecting responses with a `title`, `owner` name, and `board_id`.
