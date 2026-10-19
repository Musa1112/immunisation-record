# Immunisation record

A vaccination record for families. A parent keeps one record of the
vaccinations each of their children has had, and sees what is due
next. When a school asks for proof, the parent downloads the record as
a file. They no longer need to photograph an old paper booklet.

A practice (the doctor's surgery or clinic where the child is seen)
uses the same product. It sees which of its children are behind on
their vaccinations, so a nurse can call the family.

## What finished looks like

Anyone can open the app on their phone and:

- create an account and prove that their email address is theirs
- add a child with their date of birth
- record the vaccinations that child has already had
- see what is due next, and when
- see what is overdue, and how to catch up
- download a card to give to the school

The practice can open its own screen and see which of its children are
behind. A nurse can record a vaccination they have just given.

Finished means it is live on the internet, not just on your laptop. A
parent you have never met can use it on their phone, on mobile data.

## The road map

Eight sprints. Each sprint delivers one part of the list above. They
must be done in order, because each one builds on the one before.

| # | Sprint | What it delivers |
|---|---|---|
| 1 | Somebody the record belongs to | Parents create accounts. Practice accounts cannot be made from the sign-up form |
| 2 | One child, one history | A parent adds a child, their date of birth, and the vaccinations they have already had |
| 3 | What is due next | The schedule, and each child's timeline worked out from their date of birth |
| 4 | Late is not the same as missing | Overdue doses, a catch-up plan, and a nightly job that finds who is behind |
| 5 | The practice's list | The practice sees which of its children are behind, and records the doses it gives |
| 6 | The card the school asks for | A card the parent can download, and photos of the old paper booklet |
| 7 | Proving the address | Each account belongs to the person who reads its inbox |
| 8 | Put it where parents are | The app is live on the internet at its own address |

## The hard part

Nothing happens when a date passes. A dose becomes overdue at midnight.
No one presses a button and no request arrives, so there is nothing to
start any work. The product must still be right for the parent who
opens the page that morning. It must also be right for the practice
that is working down a list of families to call.

Sprints 3 and 4 solve this. The answer is not a date check on one
screen. It is a decision about which facts are worked out when someone
asks and which are stored. It also covers what happens when the two
disagree.

## No outside services needed to start

Email is printed to the terminal while you build. Files are kept on
your own machine. Sprint 8 replaces both with real services. Their keys
go in the environment settings, never in this repository.

## Working on it

The sprints, their briefs and their tickets are in Blacksmith
Experience. Start with sprint 1. Each sprint opens when the one before
it is finished.

Read the documents attached to a sprint before you start its first
ticket. They explain the choices you will make and the options you are
choosing between.

## What this is built with

The whole product: an API and the screens that use it.

- **Express 5** in **TypeScript** for the API.
- **Prisma** for the models and the migrations, against the schema in `prisma/schema.prisma`.
- **Zod** for validating every request, and **zod-to-openapi** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **JWT** for authentication, in `src/modules/auth`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, Prisma client, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the JWT secret, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── prisma/
│   └── schema.prisma    # your models, and the migrations they generate
├── src/
│   ├── config/          # environment, OpenAPI, Zod setup
│   ├── db/              # the Prisma client, made once
│   ├── middleware/      # authentication, validation, error handling
│   ├── modules/
│   │   └── auth/        # register, log in, refresh
│   ├── utils/           # errors, pagination, tokens
│   ├── app.ts           # where routes are mounted
│   └── index.ts
├── package.json
└── tsconfig.json

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a Zod schema or a route. |
| `blacksmith make:resource Post` | Scaffold a Prisma model, Zod schemas, a service, a controller and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run an npm script in the backend, e.g. `blacksmith backend run migrate`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Express and React project. Nothing here is a dependency on us. |
