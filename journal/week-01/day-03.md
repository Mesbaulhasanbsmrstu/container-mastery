# W1D3 — Dockerfile basics

## Recall (from memory, before notes)

## Did
- Created TaskApi with `dotnet new webapi -n TaskApi -o src\TaskApi`
- Wrote a single-stage Dockerfile (FROM, WORKDIR, COPY, RUN, ENTRYPOINT)
- Built with `docker build -f docker\Dockerfile -t taskapi:0.1 .`
- Ran with `-p 5000:8080`; `curl /weatherforecast` returned data
- Image size: ___ MB

## Broke / surprised me
- "failed to read dockerfile": Notepad saved `Dockerfile.txt`. Check with `dir`, not Explorer.
- .NET images listen on 8080, not 80.
- RUN = build time, ENTRYPOINT = every container start.

## Carry-over
- None

## Next session starts with
`docker volume create sqldata`