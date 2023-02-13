### DATA Structure

The information and structure of each CSV file is as follows:

+ ARTIFACT: containing the basic information of the software we collected whatever it plays a role of downstream or upstream. 

  ```
  GROUP_ID | ARTIFACT_ID | VERSION | LOC |  USAGE_NUM | CLASS_NUM | ID
  ```

+ CVE: containing the basic information of the CVE we collected identified by the official CVE ID.

  ```
  CVE_ID | CVSS | CWE | VUL_FUNs
  ```

+ DEP: containing the dependent relationship (downstream GAVs depend on upstream GAVs) of the artifacts based on the Maven dependency graph we constructed.

  ```
  UP_GAV_ID(referred from ARTIFACT) | DOWN_GAV_ID(referred from ARTIFACT) 
  ```

+ PATCH: containing the CVEs (identified by CVE ID), their corresponding upstream artifacts and the patches.

  ```
  CVE(referred from CVE) | Patch | AFFECT_GAV_ID(referred from ARTIFACT)
  ```

* RESPONSE: containing the downstream commits resolving (generally updating) the vulnerability of the updstream dependencies.

  ```
  CVE | Upstream_GAV | Downstream_GAV | Downstream_repo | Downstream_commit



GAV refers to GroupID, ArtifactID, Version. The GAV is used by maven as an unique identification to localize an artifact in the Maven central. Offcial definition of GAV can be find at: https://maven.apache.org/guides/mini/guide-naming-conventions.html