Submit
=============

First, login.

<h2 id="login">Login</h2>

|          |                        Login                        |
|:--------:|:---------------------------------------------------:|
|  Method  |                         POST                        |
|   Path   |                        /login                       |
|   Form   | {"Username":"packetx", "PasswordHash":"xxxxxxxxxx"} |
| Response |        Home page or {"ErrorMessage": "xxxx"}        |

You should get the correct authorization before doing anything.

`PasswordHash` is the password hash by `hash256`.

```
$ curl -k \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"Username":"packetx", "PasswordHash":"xxxxxxxxxx"}' \
    -c cookie.txt \
    https://${IP_ADDR}/login
```

<h2 id="renew">Submit</h2>

|          |             Submit             |
|:--------:|:------------------------------:|
|  Method  |              POST              |
|   Path   |          /submit/renew         |
|   Form   |    { "Base64Xml": "xxxxx" }    |
| Response | {} or {"ErrorMessage": "xxxx"} |

Second, the XML task needs encode as `base64`.

```
ENCODED=$(cat <<'EOF' | base64
<run>

<regular>

<chain>
    <in>P1</in>
    <out>P2</out>
</chain>

<chain>
    <in>P2</in>
    <out>P1</out>
</chain>

</regular>

</run>
EOF
)
```

Last, POST it.

```
$ curl -k \
    -X POST \
    -H "Content-Type: application/json" \
    -H "X-PacketX-Username: packetx" \
    -d "{ \"Base64Xml\": \"$ENCODE\" }" \
    https://${IP_ADDR}/submit/renew \
    -b cookie.txt
```

<h2>Current Task</h2>

|          |        Curent Task       |
|:--------:|:------------------------:|
|  Method  |            GET           |
|   Path   |      /submit/current     |
|   Form   |                          |
| Response | { "Base64Xml": "xxxxx" } |

```
$ curl -k \
    -X GET \
    -H "X-PacketX-Username: packetx" \
    https://${IP_ADDR}/submit/current \
    -b cookie.txt |
    python3 -c "import sys, json; print(json.load(sys.stdin)['Base64Xml'])" | \
    base64 -d

<run>
    <regular>
        <chain>
            <in>P1</in>
            <out>P2</out>
        </chain>
        <chain>
            <in>P2</in>
            <out>P1</out>
        </chain>
    </regular>
</run>
```

**Don't forget to cleanup.**

```
$ rm -f cookie.txt
```

<h2>Full Script</h2>

```
#!/bin/sh

IP_ADDR=192.168.1.140

# login
curl -k \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"Username":"packetx", "PasswordHash":"xxxxx"}' \
    -c cookie.txt \
    https://${IP_ADDR}/login

# encode as base64
ENCODED=$(cat <<'EOF' | base64
<run>

<regular>

<chain>
    <in>P1</in>
    <out>P2</out>
</chain>

<chain>
    <in>P2</in>
    <out>P1</out>
</chain>

</regular>

</run>
EOF
)

# post
curl -k \
    -X POST \
    -H "Content-Type: application/json" \
    -H "X-PacketX-Username: packetx" \
    -d "{ \"Base64Xml\": \"$ENCODED\" }" \
    https://${IP_ADDR}/submit/renew \
    -b cookie.txt

# check
curl -k \
    -X GET \
    -H "X-PacketX-Username: packetx" \
    https://${IP_ADDR}/submit/current \
    -b cookie.txt | \
    python3 -c "import sys, json; print(json.load(sys.stdin)['Base64Xml'])" | \
    base64 -d

# cleanup
rm -f cookie.txt
```
