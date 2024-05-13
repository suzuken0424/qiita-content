# qiita-content

```shell
# ファイルをrename
docker run \
       --rm \
       --mount type=bind,src=$(pwd),dst=/app \
       -w /app \
       -e QIITA_TOKEN="your token" \
       elixir:1.15.4-otp-25-slim \
       elixir bin/normalize-filenames.exs
```
