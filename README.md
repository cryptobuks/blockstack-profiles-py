# Blockstack Profiles Python

[![CircleCI](https://img.shields.io/circleci/project/blockstack/blockstack-profiles-py/master.svg)](https://circleci.com/gh/blockstack/blockstack-profiles-py)
[![PyPI](https://img.shields.io/pypi/v/blockstack-profiles.svg)](https://pypi.python.org/pypi/blockstack-profiles/)
[![PyPI](https://img.shields.io/pypi/dm/blockstack-profiles.svg)](https://pypi.python.org/pypi/blockstack-profiles/)
[![PyPI](https://img.shields.io/pypi/l/blockstack-profiles.svg)](https://pypi.python.org/pypi/blockstack-profiles/)
[![Slack](http://slack.blockstack.org/badge.svg)](http://slack.blockstack.org/)

### Installation

```bash
$ pip install blockstack-profiles
```

If you have any trouble with the installation, see [the troubleshooting guide](/troubleshooting.md) for guidance on common issues.

### Importing

```python
from blockstack_profiles import sign_token_records, validate_token_record, get_profile_from_tokens, make_zone_file_for_hosted_data
```

### Creating Profiles

```python
profile = { "name": "Naval Ravikant", "birthDate": "1980-01-01" }
profile_components = [
    {"name": "Naval Ravikant"},
    {"birthDate": "1980-01-01"}
]
```

### Tokenizing Profiles

```python
token_records = sign_token_records(profile_components, "89088e4779c49c8c3210caae38df06193359417036d87d3cc8888dcfe579905701")
```

```python
>>> print token_records
[
  {
    "decoded_token": {
      "issuedAt": "2016-03-02T18:59:29.043308", 
      "claim": {
        "name": "Naval Ravikant"
      }, 
      "expiresAt": "2017-03-02T18:59:29.043308", 
      "subject": {
        "publicKey": "02f1fd79dcd51bd017f71546ddc0fd3c8fb7de673da8661c4ceec0463dc991cc7e"
      }
    }, 
    "token": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3N1ZWRBdCI6IjIwMTYtMDMtMDJUMTg6NTk6MjkuMDQzMzA4IiwiY2xhaW0iOnsibmFtZSI6Ik5hdmFsIFJhdmlrYW50In0sImV4cGlyZXNBdCI6IjIwMTctMDMtMDJUMTg6NTk6MjkuMDQzMzA4Iiwic3ViamVjdCI6eyJwdWJsaWNLZXkiOiIwM2U5OTUzY2IxODRiMGMyNTNlMWM1YTk2ZGY0Y2I5OTMzYmY4OWVkMmRmNWJkNzliMDJmNzFjY2ZlNWVjNTAyNjgifX0.0qQbEXTsDSbswL2qfMVzMuYU503ddfclXz3ict1rh85arXX47DW51814n1OFOAzjGoeDvsQXpfG3hB2dMHuIEw", 
    "parentPublicKey": "02f1fd79dcd51bd017f71546ddc0fd3c8fb7de673da8661c4ceec0463dc991cc7e", 
    "encrypted": false, 
    "publicKey": "02f1fd79dcd51bd017f71546ddc0fd3c8fb7de673da8661c4ceec0463dc991cc7e"
  }, 
  {
    "decoded_token": {
      "issuedAt": "2016-03-02T18:59:29.043308", 
      "claim": {
        "birthDate": "1980-01-01"
      }, 
      "expiresAt": "2017-03-02T18:59:29.043308", 
      "subject": {
        "publicKey": "02f1fd79dcd51bd017f71546ddc0fd3c8fb7de673da8661c4ceec0463dc991cc7e"
      }
    }, 
    "token": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3N1ZWRBdCI6IjIwMTYtMDMtMDJUMTg6NTk6MjkuMDQzMzA4IiwiY2xhaW0iOnsiYmlydGhEYXRlIjoiMTk4MC0wMS0wMSJ9LCJleHBpcmVzQXQiOiIyMDE3LTAzLTAyVDE4OjU5OjI5LjA0MzMwOCIsInN1YmplY3QiOnsicHVibGljS2V5IjoiMDNlOTk1M2NiMTg0YjBjMjUzZTFjNWE5NmRmNGNiOTkzM2JmODllZDJkZjViZDc5YjAyZjcxY2NmZTVlYzUwMjY4In19.m-v3mrPtXaNSltBvWfOLnpPerIxJhQQOt0-x-Lyw1A-iGp_dq8TPLrYGqo4UfcBfqva52-N5eSCN6c1pKgSLDQ", 
    "parentPublicKey": "02f1fd79dcd51bd017f71546ddc0fd3c8fb7de673da8661c4ceec0463dc991cc7e", 
    "encrypted": false, 
    "publicKey": "02f1fd79dcd51bd017f71546ddc0fd3c8fb7de673da8661c4ceec0463dc991cc7e"
  }
]
```

### Verifying Token Records

#### Verifying Against Public Keys

```python
public_key = "030589ee559348bd6a7325994f9c8eff12bd5d73cc683142bd0dd1a17abc99b0dc"
decoded_token = verify_token_record(token_records[0], public_key)
```

#### Verifying Against Addresses

```python
address = "1KbUJ4x8epz6QqxkmZbTc4f79JbWWz6g37"
decoded_token = verify_token_record(token_records[0], address)
```

### Recovering Profiles

```python
profile = get_profile_from_tokens(profile_tokens, "02f1fd79dcd51bd017f71546ddc0fd3c8fb7de673da8661c4ceec0463dc991cc7e")
```

```
>>> print profile
{
  "name": "Naval Ravikant", 
  "birthDate": "1980-01-01"
}
```

### Creating Zone Files

```python
zone_file = make_zone_file_for_hosted_data("naval.id", "https://mq9.s3.amazonaws.com/naval.id/profile.json")
```

```
$ORIGIN naval.id
$TTL 3600
@ IN URI "https://mq9.s3.amazonaws.com/naval.id/profile.json"
```
