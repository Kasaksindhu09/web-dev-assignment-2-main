# Student Management API

A simple REST API built with Node.js and Express. Data is stored in memory, so it resets when the server restarts.

## Setup

```bash
npm install
npm start          # runs on http://localhost:3000
npm run dev        # auto-restart with nodemon
```

Set a different port with the `PORT` environment variable, e.g. `PORT=5000 npm start`.

## Endpoints

| Method | URL                | Description                          |
| ------ | ------------------ | ------------------------------------ |
| GET    | `/`                | Health check                         |
| GET    | `/students`        | List students (supports query params)|
| GET    | `/students/:id`    | Get one student                      |
| POST   | `/students`        | Create a student (`name`, `course`)  |
| PUT    | `/students/:id`    | Replace a student (both fields needed) |
| PATCH  | `/students/:id`    | Update one or more fields            |
| DELETE | `/students/:id`    | Delete a student                     |

### Query params for `GET /students`

- `course=BCA` filter by course (case-insensitive)
- `name=rah` search by part of the name (case-insensitive)
- `sort=name|course|id` and `order=asc|desc`
- `page=1&limit=5` pagination. When used, the response is
  `{ data, page, limit, total, totalPages }` instead of a plain array.

### Examples

```bash
curl "http://localhost:3000/students?course=BCA&sort=name&order=desc"
curl -X POST http://localhost:3000/students -H "Content-Type: application/json" -d '{"name":"Neha","course":"BCA"}'
curl -X PATCH http://localhost:3000/students/1 -H "Content-Type: application/json" -d '{"course":"MCA"}'
```

## Error responses

All errors return JSON with a `message` field. Validation errors also include an `errors` array.
Status codes used: `400` (bad input or invalid id), `404` (not found), `500` (server error).
