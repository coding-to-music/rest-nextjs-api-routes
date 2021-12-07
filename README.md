# Fullstack Example with Next.js (REST API)

This example shows how to implement a **fullstack app in TypeScript with [Next.js](https://nextjs.org/)** using [React](https://reactjs.org/) (frontend), [Express](https://expressjs.com/) and [Prisma Client](https://www.prisma.io/docs/reference/tools-and-interfaces/prisma-client) (backend). It uses a SQLite database file with some initial dummy data which you can find at [`./prisma/dev.db`](./prisma/dev.db).

## Getting started

### 1. Download example and install dependencies

Download this example:

```
curl https://codeload.github.com/prisma/prisma-examples/tar.gz/latest | tar -xz --strip=2 prisma-examples-latest/typescript/rest-nextjs-api-routes
```

Install npm dependencies:

```
cd rest-nextjs-api-routes
npm install
```

<details><summary><strong>Alternative:</strong> Clone the entire repo</summary>

Clone this repository:

```
git clone git@github.com:prisma/prisma-examples.git --depth=1
```

Install npm dependencies:

```java
cd prisma-examples/typescript/rest-nextjs-api-routes
npm install --force
```

Output
```java
npm WARN using --force Recommended protections disabled.
npm WARN ERESOLVE overriding peer dependency
npm WARN ERESOLVE overriding peer dependency
npm WARN While resolving: next@12.0.7
npm WARN Found: react-dom@17.0.0
npm WARN node_modules/react-dom
npm WARN   peer react-dom@"^17.0.2" from @next/react-dev-overlay@12.0.7
npm WARN   node_modules/next/node_modules/@next/react-dev-overlay
npm WARN     @next/react-dev-overlay@"12.0.7" from next@12.0.7
npm WARN     node_modules/next
npm WARN 
npm WARN Could not resolve dependency:
npm WARN peer react-dom@"^17.0.2 || ^18.0.0-0" from next@12.0.7
npm WARN node_modules/next
npm WARN   next@"12.0.7" from the root project
npm WARN 
npm WARN Conflicting peer dependency: react-dom@17.0.2
npm WARN node_modules/react-dom
npm WARN   peer react-dom@"^17.0.2 || ^18.0.0-0" from next@12.0.7
npm WARN   node_modules/next
npm WARN     next@"12.0.7" from the root project
npm WARN ERESOLVE overriding peer dependency
npm WARN While resolving: react-dom@17.0.0
npm WARN Found: react@17.0.2
npm WARN node_modules/react
npm WARN   react@"17.0.2" from the root project
npm WARN   5 more (next, react-markdown, @next/react-dev-overlay, ...)
npm WARN 
npm WARN Could not resolve dependency:
npm WARN peer react@"17.0.0" from react-dom@17.0.0
npm WARN node_modules/react-dom
npm WARN   peer react-dom@"^17.0.2" from @next/react-dev-overlay@12.0.7
npm WARN   node_modules/next/node_modules/@next/react-dev-overlay
npm WARN 
npm WARN Conflicting peer dependency: react@17.0.0
npm WARN node_modules/react
npm WARN   peer react@"17.0.0" from react-dom@17.0.0
npm WARN   node_modules/react-dom
npm WARN     peer react-dom@"^17.0.2" from @next/react-dev-overlay@12.0.7
npm WARN     node_modules/next/node_modules/@next/react-dev-overlay

added 382 packages, and audited 383 packages in 41s

101 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```


</details>

### 2. Create and seed the database

Run the following command to create your SQLite database file. This also creates the `User` and `Post` tables that are defined in [`prisma/schema.prisma`](./prisma/schema.prisma):

```
npx prisma migrate dev --name init
```

Output
```java
Prisma schema loaded from prisma/schema.prisma
Datasource "db": SQLite database "dev.db" at "file:./dev.db"

Applying migration `20211206234407_init`

The following migration(s) have been created and applied from new schema changes:

migrations/
  └─ 20211206234407_init/
    └─ migration.sql

Your database is now in sync with your schema.

✔ Generated Prisma Client (3.5.0) to ./node_modules/@prisma/client in 206ms
```

Now, seed the database with the sample data in [`prisma/seed.ts`](./prisma/seed.ts) by running the following command:

```
npx prisma db seed
```

Output
```java
Running seed command `ts-node prisma/seed.ts` ...
Start seeding ...
Created user with id: 1
Created user with id: 2
Created user with id: 3
Seeding finished.

🌱  The seed command has been executed.
┌─────────────────────────────────────────────────────────┐
│  Update available 3.5.0 -> 3.6.0                        │
│  Run the following to update                            │
│    npm i --save-dev prisma@latest                       │
│    npm i @prisma/client@latest                          │
└─────────────────────────────────────────────────────────┘
```

### Change source= to children= in a few places

[react-markdown] Warning: please use `children` instead of `source` (see <https://github.com/remarkjs/react-markdown/blob/main/changelog.md#change-source-to-children> for more info)

### 3. Start the app

```
npm run dev
```

The app is now running, navigate to [`http://localhost:3000/`](http://localhost:3000/) in your browser to explore its UI.

<details><summary>Expand for a tour through the UI of the app</summary>

<br />

**Blog** (located in [`./pages/index.tsx`](./pages/index.tsx)

![](https://imgur.com/eepbOUO.png)

**Signup** (located in [`./pages/signup.tsx`](./pages/signup.tsx))

![](https://imgur.com/iE6OaBI.png)

**Create post (draft)** (located in [`./pages/create.tsx`](./pages/create.tsx))

![](https://imgur.com/olCWRNv.png)

**Drafts** (located in [`./pages/drafts.tsx`](./pages/drafts.tsx))

![](https://imgur.com/PSMzhcd.png)

**View post** (located in [`./pages/p/[id].tsx`](./pages/p/[id].tsx)) (delete or publish here)

![](https://imgur.com/zS1B11O.png)

</details>

## Using the REST API

You can also access the REST API of the API server directly. It is running on the same host machine and port and can be accessed via the `/api` route (in this case that is `localhost:3000/api/`, so you can e.g. reach the API with [`localhost:3000/api/feed`](http://localhost:3000/api/feed)).

### `GET`

- `/api/post/:id`: Fetch a single post by its `id`
- `/api/feed`: Fetch all _published_ posts
- `/api/filterPosts?searchString={searchString}`: Filter posts by `title` or `content`

### `POST`

- `/api/post`: Create a new post
  - Body:
    - `title: String` (required): The title of the post
    - `content: String` (optional): The content of the post
    - `authorEmail: String` (required): The email of the user that creates the post
- `/api/user`: Create a new user
  - Body:
    - `email: String` (required): The email address of the user
    - `name: String` (optional): The name of the user

### `PUT`

- `/api/publish/:id`: Publish a post by its `id`

### `DELETE`
  
- `/api/post/:id`: Delete a post by its `id`

## Evolving the app

Evolving the application typically requires three steps:

1. Migrate your database using Prisma Migrate
1. Update your server-side application code
1. Build new UI features in React

For the following example scenario, assume you want to add a "profile" feature to the app where users can create a profile and write a short bio about themselves.

### 1. Migrate your database using Prisma Migrate

The first step is to add a new table, e.g. called `Profile`, to the database. You can do this by adding a new model to your [Prisma schema file](./prisma/schema.prisma) file and then running a migration afterwards:

```diff
// schema.prisma

model Post {
  id        Int     @default(autoincrement()) @id
  title     String
  content   String?
  published Boolean @default(false)
  author    User?   @relation(fields: [authorId], references: [id])
  authorId  Int
}

model User {
  id      Int      @default(autoincrement()) @id 
  name    String? 
  email   String   @unique
  posts   Post[]
+ profile Profile?
}

+model Profile {
+  id     Int     @default(autoincrement()) @id
+  bio    String?
+  userId Int     @unique
+  user   User    @relation(fields: [userId], references: [id])
+}
```

Once you've updated your data model, you can execute the changes against your database with the following command:

```
npx prisma migrate dev
```

Output
```java
Prisma schema loaded from prisma/schema.prisma
Datasource "db": SQLite database "dev.db" at "file:./dev.db"

✔ Enter a name for the new migration: … profile
Applying migration `20211207002008_profile`

The following migration(s) have been created and applied from new schema changes:

migrations/
  └─ 20211207002008_profile/
    └─ migration.sql

Your database is now in sync with your schema.

✔ Generated Prisma Client (3.5.0) to ./node_modules/@prisma/client in 353ms
```

### 2. Update your application code

You can now use your `PrismaClient` instance to perform operations against the new `Profile` table. Here are some examples:

#### Create a new profile for an existing user

```ts
const profile = await prisma.profile.create({
  data: {
    bio: "Hello World",
    user: {
      connect: { email: "alice@prisma.io" },
    },
  },
});
```

#### Create a new user with a new profile

```ts
const user = await prisma.user.create({
  data: {
    email: "john@prisma.io",
    name: "John",
    profile: {
      create: {
        bio: "Hello World",
      },
    },
  },
});
```

#### Update the profile of an existing user

```ts
const userWithUpdatedProfile = await prisma.user.update({
  where: { email: "alice@prisma.io" },
  data: {
    profile: {
      update: {
        bio: "Hello Friends",
      },
    },
  },
});
```


### 3. Build new UI features in React

Once you have added a new endpoint to the API (e.g. `/api/profile` with `/POST`, `/PUT` and `GET` operations), you can start building a new UI component in React. It could e.g. be called `profile.tsx` and would be located in the `pages` directory.

In the application code, you can access the new endpoint via `fetch` operations and populate the UI with the data you receive from the API calls.


## Switch to another database (e.g. PostgreSQL, MySQL, SQL Server, MongoDB)

If you want to try this example with another database than SQLite, you can adjust the the database connection in [`prisma/schema.prisma`](./prisma/schema.prisma) by reconfiguring the `datasource` block. 

Learn more about the different connection configurations in the [docs](https://www.prisma.io/docs/reference/database-reference/connection-urls).

<details><summary>Expand for an overview of example configurations with different databases</summary>

### PostgreSQL

For PostgreSQL, the connection URL has the following structure:

```prisma
datasource db {
  provider = "postgresql"
  url      = "postgresql://USER:PASSWORD@HOST:PORT/DATABASE?schema=SCHEMA"
}
```

Here is an example connection string with a local PostgreSQL database:

```prisma
datasource db {
  provider = "postgresql"
  url      = "postgresql://janedoe:mypassword@localhost:5432/notesapi?schema=public"
}
```

### MySQL

For MySQL, the connection URL has the following structure:

```prisma
datasource db {
  provider = "mysql"
  url      = "mysql://USER:PASSWORD@HOST:PORT/DATABASE"
}
```

Here is an example connection string with a local MySQL database:

```prisma
datasource db {
  provider = "mysql"
  url      = "mysql://janedoe:mypassword@localhost:3306/notesapi"
}
```

### Microsoft SQL Server

Here is an example connection string with a local Microsoft SQL Server database:

```prisma
datasource db {
  provider = "sqlserver"
  url      = "sqlserver://localhost:1433;initial catalog=sample;user=sa;password=mypassword;"
}
```

### MongoDB

Here is an example connection string with a local MongoDB database:

```prisma
datasource db {
  provider = "mongodb"
  url      = "mongodb://USERNAME:PASSWORD@HOST/DATABASE?authSource=admin&retryWrites=true&w=majority"
}
```
Because MongoDB is currently in [Preview](https://www.prisma.io/docs/about/releases#preview), you need to specify the `previewFeatures` on your `generator` block:

```
generator client {
  provider        = "prisma-client-js"
  previewFeatures = ["mongodb"]
}
```
</details>

## Next steps

- Check out the [Prisma docs](https://www.prisma.io/docs)
- Share your feedback in the [`prisma2`](https://prisma.slack.com/messages/CKQTGR6T0/) channel on the [Prisma Slack](https://slack.prisma.io/)
- Create issues and ask questions on [GitHub](https://github.com/prisma/prisma/)
- Watch our biweekly "What's new in Prisma" livestreams on [Youtube](https://www.youtube.com/channel/UCptAHlN1gdwD89tFM3ENb6w)
