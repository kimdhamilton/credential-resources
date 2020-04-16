Experimenting with ways of expressing VC payloads

## Use Cases
1. Within a VC, embed a link to file (which could be a local file), along with content integrity check. We were trying to address multiple problems:
    - there are multiple viable representations of the content, e.g. machine- and human- readable
    - separation of concerns -- still needed image for bridging to existing systems, but that was different from the core data. 
2. (ILR Wallet) Embed payload with different content types



## Possible Approaches

In Blockcerts v1 we did a terrible approach of embedding html to preserve display, but it's still there.

I proposed several iterations of proposed improvements. The last written was [resource integrity proofs (RIPs)](https://github.com/WebOfTrustInfo/rwot7-toronto/blob/master/final-documents/resource-integrity-proofs.md), it was never implemented (and there is a subsequent possible improvement, described below).


Tl;dr, the RIP paper proposed this:
```
"verifiableDisplay": {
  "id": "https://raw.githubusercontent.com/WebOfTrustInfo/rwot7/master/draft-documents/images/exampleVerifiableDisplay.png",
  "proof": {
    "type": ["ResourceIntegrityProof", "Multihash2018"],
    "contentType: "image/png",
    "multiDigest": "zQmUvZSaVzgjVHCDDDAoNNBgpiAkN6wKmCcD37vvnmoKq6e"
  }
}
```

Since the RIP paper was written, there's been a proposal [hashlinks](https://w3c-ccg.github.io/hashlink/), which makes the above more compact by merging the link and the digest. So you could collapse it to something like this:

```
verifiableDisplay: {
  "id": "ni:....", <-- hashlink has the link and the digest
  "contentType": "image/png",
}
```

Note that, if multiple viable displays (or representations) is a desired use case, you might instead want something like this:

```
verifiableDisplay: [
    {
        "id": "ni:....", <-- hashlink has the link and the digest
        "contentType": "image/png",
        "additionalmetadata": "..."
    }, {},
]
```


## Use Case 2 Current Proposal:
```
“payloadFormat”: “xml”,
    “payloadCompression”: “gzip”,
    “payloadEncoding”: “base64”,
    “payload”: “PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHJvb3Q6Um9vdFRhZ09mRG9jdW1lbnQKeG1sbnM6cm9vdD0idXJuOm9yZzpwZXNjOm1lc3NhZ2U6dGhpc21lc3NhZ2U6djEuMC4wIgp4bWxuczpzZWN0b3I9InVybjpvcmc6cGVzYzpzZWN0b3I6dGhpc3NlY3Rvcjp2MS4wLjAiCnhtbG5zOmNvcmU9InVybjpvcmc6cGVzYzpjb3JlOkNvcmVNYWluOlYxLjEzIgp4bWxuczp4c2k9Imh0dHA6Ly93d3cudzMub3JnLzIwMDEvWE1MU2NoZW1hLWluc3RhbmNlIgp4c2k6c2NoZW1hTG9jYXRpb249InVybjpvcmc6cGVzYzptZXNzYWdlOnRoaXNtZXNzYWdlOnYxLjAuMApNZXNzYWdlU3BlYy54c2QiPgo8RG9jdW1lbnRJRD4yMDAzLTA4LTE3VDA5OjMwOjQ3LTA1OjAwQmF0Y2hJRDwvRG9jdW1lbnRJRD4KPCEtLSBhbGwgb3RoZXIgZWxlbWVudHMgZG8gbm90IG5lZWQgdG8gYmUgcXVhbGlmaWVkIGV4Y2VwdCBmb3IgdGhlIHJvb3QgLS0KPgo8L3Jvb3Q6Um9vdFRhZ09mRG9jdW1lbnQ+”
}
```


## Notes and Questions

- Do we think the multiple viable displays use case is a core use case?
- Are there cases where you might want to link to the content instead of embed it?
