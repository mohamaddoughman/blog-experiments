# blog-experiments

Small, runnable .NET labs behind the posts on **[mohamaddoughman.dev](https://mohamaddoughman.dev)**.

Each lab answers one backend question by building the smallest thing that shows the answer, breaking it on
purpose, and measuring it. The blog post explains what happened; this repo lets you run it yourself and check
the numbers.

## Labs

| # | Lab | Question | Post |
| --- | --- | --- | --- |
| | *Coming soon* | | |

Labs follow a learning roadmap: data and concurrency first, then performance, caching, APIs and resilience,
design and architecture, messaging and distributed systems, and finally case studies of how well-known
companies built their systems.

## Run a lab

You need the [.NET 10 SDK](https://dotnet.microsoft.com/download) and [Docker](https://www.docker.com/).

```bash
# 1. One time: create your local settings (only used by the local containers)
cp .env.example .env

# 2. Start SQL Server and Redis (shared by all labs)
docker compose up -d

# 3. Run a lab: each folder's README says exactly what to run and what to watch for
cd labs/01-database-indexes
dotnet run
```

Stop the containers with `docker compose down` (add `-v` to also delete the data).

## How a lab is organized

```
labs/NN-topic/
├── README.md     # question, how to run, what to watch, what to try, results
├── src/          # the smallest code that shows the answer
└── load/         # k6 scripts or benchmarks, when the answer is about numbers
```

Every README follows the same structure: **Question → Run → What to watch → Try changing → Results**.
See [`labs/_template/README.md`](labs/_template/README.md).

## Notes

- Everything runs locally against throwaway containers. No cloud accounts, no real data, no secrets.
- Numbers depend on your machine. Compare the variants of a lab with each other, not with my absolute numbers.
- Found a mistake or got different results? Open an issue: that's how experiments get better.

## License

[MIT](LICENSE)
