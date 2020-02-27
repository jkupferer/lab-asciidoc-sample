LAB - AsciiDoc Sample
=====================

Sample workshop content using AsciiDoc formatting for pages.

Build
=====

```
NAME=bookbag
```

```
oc process --local -f build-template.yaml \
-p NAME="${NAME}" \
| oc apply -f -
```

```
oc start-build ${NAME} --from-dir=. --follow
```

```
cat >workshop-vars.json <<EOF
{
  "GUID": "abcde",
  "random_string": "p4ssw0rd",
  "user_info_messages": "hello\nworld\n"
}
EOF
```

```
oc process --local -f deploy-template.yaml \
-p NAME="${NAME}" \
-p IMAGE="$(oc get is ${NAME} -o jsonpath='{.status.tags[?(@.tag=="latest")].items[0].dockerImageReference}')" \
-p WORKSHOP_VARS="$(cat workshop-vars.json)" \
| oc apply -f -
```
