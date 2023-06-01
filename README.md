# Plaid quickstart

This repository accompanies Plaid's [**quickstart guide**][quickstart], using only the Python back-end example.

Here you'll find a full example integration app using Plaid's [**Python client library**][library].

This Quickstart is designed to showcase several basic Plaid APIs, against a vanilla JS frontend. 

If you prefer a React frontend platform, use [the original version of this repo](https://github.com/plaid/quickstart), which integrates one. For a more minimal backend in one language with one endpoint, see the [Tiny Quickstart](https://github.com/plaid/tiny-quickstart), which shows a simpler backend and is available for JavaScript, Next.js, React, and React Native frontends.

## Table of contents

<!-- toc -->

- [1. Clone the repository](#1-clone-the-repository)
- [2. Set up your environment variables](#2-set-up-your-environment-variables)
- [3. Run the quickstart](#3-run-the-quickstart)
  - [Pre-requisites](#pre-requisites)
  - [1. Running the backend](#1-running-the-backend)
    - [Python](#python)
- [Test credentials](#test-credentials)
- [Troubleshooting](#troubleshooting)
- [Testing OAuth](#testing-oauth)

<!-- tocstop -->

## 1. Clone the repository

Using https:

```bash
git clone https://github.com/plaid/quickstart
cd quickstart
```

Alternatively, if you use ssh:

```bash
git clone git@github.com:plaid/quickstart.git
cd quickstart
```

## 2. Set up your environment variables

```bash
cp .env.example .env
```

Copy `.env.example` to a new file called `.env` and fill out the environment variables inside. At
minimum `PLAID_CLIENT_ID`, `PLAID_SECRET`, and `PLAID_REDIRECT_URI` must be filled out. Get your Client ID and secrets from
the dashboard: https://dashboard.plaid.com/account/keys

We suggest putting `http://localhost:3000` for the `PLAID_REDIRECT_URI`.

> NOTE: `.env` files are a convenient local development tool. Never run a production application
> using an environment file with secrets in it.

## 3. Add `PLAID_REDIRECT_URI` to API Dashboard

In [the Plaid API dashboard](https://dashboard.plaid.com/team/api) click "configure" next to "Allowed redirect URIs" and add your `PLAID_REDIRECT_URI` there.

## 4. Run the Quickstart

### Pre-requisites

- The language you intend to use is installed on your machine and available at your command line: python >= 3.8
- Your environment variables populated in `.env`

### 1. Running the backend

Once started with one of the commands below, the quickstart will be running on http://localhost:8000 for the backend. Enter the additional commands in step 2 to run the frontend which will run on http://localhost:3000.

```bash
cd backend
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
python server.py
```

If you get this error message:

```txt
ssl.SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:749)
```

You may need to install SSL certificates. 

On MacOS:

```bash
open /Applications/Python\ 3.<your python version>/Install\ Certificates.command
```

On Linux:

```bash
sudo update-ca-certificates --fresh
export SSL_CERT_DIR=/etc/ssl/certs
```

or 

```bash
cd $HOME
wget --quiet https://curl.haxx.se/ca/cacert.pem
export SSL_CERT_FILE=$HOME/cacert.pem
```

On Windows:

```powershell
cd $HOME
wget --quiet https://curl.haxx.se/ca/cacert.pem
set SSL_CERT_FILE=$HOME/cacert.pem
```

### Running the frontend

```bash
cd frontend
python3 -m http.server 8080
```

## Test credentials

In Sandbox, you can log in to any supported institution (except Capital One) using any username or password. If prompted to enter a 2-factor authentication code, enter anything you want and it'll work.

In Development or Production, use real-life credentials.

## Troubleshooting

### Can't get a link token, or API calls are 400ing

View the server logs to see the associated error message with detailed troubleshooting instructions. If you can't view logs locally, view them via the [Dashboard activity logs](https://dashboard.plaid.com/activity/logs). 

### "Connectivity not supported"

If you get a "Connectivity not supported" error after selecting a financial institution in Link, you probably specified some products in your .env file that the target financial institution doesn't support. Remove the unsupported products and try again.

### "You need to update your app" or "institution not supported"

If you get a "You need to update your app" or "institution not supported" error after selecting a financial institution in Link, you're probably running the Quickstart in Development (or Production) and attempting to link an institution, such as Chase or Wells Fargo, that requires an OAuth-based connection. In order to make OAuth connections to US-based institutions in Development or Production, you must have Production access approval, and certain institutions may also require additional steps. To use this institution, [apply for Production access](https://dashboard.plaid.com/overview/production) and see the [OAuth insitutions page](https://dashboard.plaid.com/team/oauth-institutions) for any other required steps.

### "oauth uri does not contain a valid oauth_state_id query parameter"

If you get the console error "oauth uri does not contain a valid oauth_state_id query parameter", you are attempting to initialize Link with a redirect uri when it is not necessary to do so. The `receivedRedirectUri` should not be set when initializing Link for the first time. It is used when initializing Link for the second time, after returning from the OAuth redirect.


[quickstart]: https://plaid.com/docs/quickstart
[library]: https://github.com/plaid/plaid-python
[payment-initiation]: https://plaid.com/docs/payment-initiation/
[dashboard-api-section]: https://dashboard.plaid.com/team/api
[contact-sales]: https://plaid.com/contact
