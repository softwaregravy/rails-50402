# README

### How I reproduced
1. fresh project with `.gemset`, `.ruby-version`, and `.gitignore`
2. `gem install rails -v 7.1.2`
3. `rails new . -d postgresql`
4. `docker build --progress=plain -t rails-50402 .`

```bash
$ cat config/master.key
56c2424517b428203a33f2b7b019daaa
$ docker run -p 3000:3000 -e RAILS_MASTER_KEY="56c2424517b428203a33f2b7b019daaa" -e DATABASE_URL=postgresql://johnhinnegan:@host.docker.internal/rails_50402_development rails-50402
=> Booting Puma
=> Rails 7.1.2 application starting in production
=> Run `bin/rails server --help` for more startup options
[1] Puma starting in cluster mode...
[1] * Puma version: 6.4.0 (ruby 3.2.2-p53) ("The Eagle of Durango")
[1] *  Min threads: 5
[1] *  Max threads: 5
[1] *  Environment: production
[1] *   Master PID: 1
[1] *      Workers: 8
[1] *     Restarts: (✔) hot (✔) phased
[1] * Listening on http://0.0.0.0:3000
[1] Use Ctrl-C to stop
[1] - Worker 0 (PID: 11) booted in 0.01s, phase: 0
[1] - Worker 1 (PID: 12) booted in 0.01s, phase: 0
[1] - Worker 2 (PID: 15) booted in 0.0s, phase: 0
[1] - Worker 3 (PID: 21) booted in 0.0s, phase: 0
[1] - Worker 4 (PID: 28) booted in 0.0s, phase: 0
[1] - Worker 5 (PID: 35) booted in 0.0s, phase: 0
[1] - Worker 6 (PID: 44) booted in 0.0s, phase: 0
[1] - Worker 7 (PID: 56) booted in 0.0s, phase: 0
^C[1] - Gracefully shutting down workers...
[1] === puma shutdown: 2023-12-26 16:05:23 +0000 ===
[1] - Goodbye!
Exiting
$ docker run -p 3000:3000 -e RAILS_MASTER_KEY="typo" -e DATABASE_URL=postgresql://johnhinnegan:@host.docker.internal/rails_50402_development rails-50402
bin/rails aborted!
ArgumentError: key must be 16 bytes (ArgumentError)

        cipher.key = @secret
                     ^^^^^^^
/rails/config/environment.rb:5:in `<main>'
Tasks: TOP => db:prepare => db:load_config => environment
(See full trace by running task with --trace)
```
