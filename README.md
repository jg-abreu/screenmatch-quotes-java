# ScreenMatch Quotes

![Java 17](https://img.shields.io/badge/Java-17-ED8B00?logo=openjdk&logoColor=white)
![Spring Boot 4](https://img.shields.io/badge/Spring_Boot-4-6DB33F?logo=springboot&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A REST API in Spring Boot that serves a random famous quote from movies and TV shows, stored in PostgreSQL and displayed by a small HTML/CSS/JavaScript front end.

## Features

- **Random quote endpoint** — `GET /series/frases` returns one quote picked at random by the database
- **PostgreSQL persistence** — quotes are stored as JPA entities (title, quote, character and poster image)
- **Front end integration** — the page fetches a new quote on load and every time you click the button
- **DTO response** — the entity is mapped to a record so the API returns exactly the fields the front end expects

## How it works

- `QuoteRepository` uses a native query (`ORDER BY RANDOM() LIMIT 1`) so the database picks the random row instead of loading every quote into memory.
- `QuoteService` maps the entity to `QuoteDTO`, and `QuoteController` exposes it with CORS enabled so the page can call the API from another origin.
- The JSON fields (`titulo`, `frase`, `personagem`, `poster`) follow the contract of the front-end template provided by the challenge.

Example response:

```json
{
  "titulo": "Breaking Bad",
  "frase": "I am the one who knocks.",
  "personagem": "Walter White",
  "poster": "https://..."
}
```

## Tech stack

**Back end:** Java 17 · Spring Boot 4 (Web MVC, Data JPA) · Hibernate · PostgreSQL · Maven
**Front end:** HTML · CSS · JavaScript (ES modules, `fetch`)

## Running

Requirements: Java 17+ and a running PostgreSQL server.

1. Create the database:
   ```sql
   CREATE DATABASE gerador_frases;
   ```

2. Set the connection through environment variables (the defaults are shown in parentheses):

   | Variable | Default |
   |---|---|
   | `DB_HOST` | `localhost:5432` |
   | `DB_USER` | `postgres` |
   | `DB_PASSWORD` | `postgres` |

3. Start the API:
   ```bash
   git clone https://github.com/jg-abreu/screenmatch-quotes-java.git
   cd screenmatch-quotes-java
   ./mvnw spring-boot:run        # http://localhost:8080
   ```
   Hibernate creates the `quotes` table on the first run.

4. Add some quotes (the table starts empty):
   ```sql
   INSERT INTO quotes (title, quote, character, poster) VALUES
   ('Breaking Bad', 'I am the one who knocks.', 'Walter White', '<poster image URL>');
   ```

5. Serve the front end. It uses ES modules, so it has to be opened through a local server rather than by double-clicking `index.html`:
   ```bash
   cd frontend
   python -m http.server 5500     # then open http://localhost:5500
   ```
   The VS Code *Live Server* extension works too.

## Project structure

```
├── src/main/java/br/com/alura/gerador_de_frases
│   ├── controller/   REST endpoint
│   ├── service/      entity → DTO mapping
│   ├── repository/   native query for a random quote
│   ├── model/        Quote entity
│   └── dto/          QuoteDTO record
└── frontend/         HTML, CSS and JavaScript page
```

## Credits

Built as the solution to a challenge from [Alura](https://www.alura.com.br)'s Java course on building a first REST API and connecting it to a front end. The front-end template in `frontend/` was provided by Alura; the API is my own implementation.

## License

The API code is released under the [MIT License](LICENSE). The front-end template belongs to Alura.

## Author

**João Abreu** — back-end developer and Computer Science student in Rio de Janeiro, Brazil.

[GitHub](https://github.com/jg-abreu) · [LinkedIn](https://www.linkedin.com/in/jo%C3%A3o-abreuuu) · jgabreudev@gmail.com
