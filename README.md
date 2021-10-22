# Nymeria

The official Golang package and command line tool to interact with the Nymeria
service.

![Nymeria makes finding contact details a breeze.](https://www.nymeria.io/marquee.png)

## API

#### Set and Check an API Key.

```go
nymeria.SetAuth("ny_your-api-key")

if err := nymeria.CheckAuthentication(); err == nil {
  log.Println("OK!")
}
```

All API endpoints assume an auth key has been set. You should set the auth key
early in your program. The key will automatically be added to all future
requests.

#### Verify an Email Address

```go
if v, err := nymeria.Verify("someone@somewhere.com"); err == nil {
  log.Println(v.Data.Result)
}
```

At this time only professional email addresses are supported by the API.

#### Enrich a Profile

```go
if v, err := nymeria.Enrich("github.com/someone"); err == nil {
  if v.Status == "success" {
    log.Println(v.Data.Emails)
  }
}
```

The enrich API works on a profile by profile basis. If you need to enrich
multiple profiles at once you can use the bulk enrichment API.

#### Bulk Enrichment of Profiles

```go
// Up to 100 URLs will be enriched at a time.
urls := []string{
  "github.com/someone",
  "linkedin.com/in/someoneelse",
}

if v, err := nymeria.BulkEnrich(urls...); err == nil {
  if v.Status == "success" {
    for _, match := range v.Data {
      log.Println(match.Result.Emails)
    }
  }
}
```

## Command Line Tool

The command line tool enables you to quickly test the Nymeria API.

#### Installation

You can install the command line tool with `go get`.

```bash
$ go install github.com/nymeriaio/nymeria.go/cmd/nymeria-cli@latest
```

#### Set an API Key

```bash
$ nymeria-cli --auth ny_abc-123-456
```

The API key will be cached for future commands.

#### Purge all cached data.

```bash
$ nymeria-cli --purge
```

#### Check Authentication

To quickly check your auth key you can run the following:

```bash
$ nymeria-cli --check-auth
```

#### Verify an Email Address

```bash
$ nymeria-cli --verify someone@somewhere.com
```

#### Enrich a Profile

```bash
$ nymeria-cli --enrich github.com/someone
```

#### Bulk Enrich Profiles

```bash
$ nymeria-cli --bulkenrich github.com/someone,linkedin.com/in/someoneelse
```

## License

MIT License

Copyright (c) 2021, Nymeria LLC.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
