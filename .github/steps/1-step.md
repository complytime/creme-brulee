## Step 1: Initialize the trestle workspace with trestle-bot 

_This will house the authored OSCAL content_

### 📖 Theory: Author OSCAL Content and Update it in your [trestle-workspace](https://github.com/hbraswelrh/trestle-workspace/tree/main)

The [trestle-workspace](https://github.com/hbraswelrh/trestle-workspace/tree/main) is a public template that can be leveraged with trestle-bot in GitHub CI. The file structure layout is seen in the tree below. 

### ⌨️ Activity: Interact with the trestle-workspace

To interact with the trestle workspace, you will need to create your own copy of the [trestle-workspace](https://github.com/hbraswelrh/trestle-workspace/tree/main) public template. This will allow you to have your own trestle-workspace directly out of the box. 

**Step 1🖱️:** Navigate to the [trestle-workspace](https://github.com/hbraswelrh/trestle-workspace/tree/main). Click the green box in the upper right corner that says "Use this template."

<img src="https://github.com/user-attachments/assets/7f0d54c1-daef-488a-b8e4-0887cf7e491a" alt="text" width="375" height="200">

**Step 2🖱️🟩:** Click "Create a new repository." This step is comparable to the "Make a Copy" functionality when duplicating a Google Doc. 

<img src="https://github.com/user-attachments/assets/0ce8f4a7-453d-408e-8174-75cec1cc507a" alt="text" width="250" height="85">

**Step 3🖱️🟩:** Ensure that you are the repository owner and create a name for your new trestle-workspace template. Then, click "Create Repository."

_You can choose to make the repository Public or Private_

<img src="https://github.com/user-attachments/assets/bb4ffe1a-20f0-4ab7-abd7-d76a2eb10573" alt="text" width="250" height="250">

**Step 4🪄:** Your repository has now been created and will be available under `{YOUR_GITHUB_USERNAME}/{TRESTLE_WORKSPACE_NAME}`

<img src="https://github.com/user-attachments/assets/f7fd13d0-4e58-4613-851c-57bb09f5b7f2" alt="text" width="375" height="200">


### Trestle Workspace Context: 

#### Running trestle-bot commands in GitHub Actions 

The trestle-workspace supports running trestlebot commands in GitHub Actions. The available actions are autosync, create, and rules-transform. The workflows can be seen [here](https://github.com/hbraswelrh/trestle-workspace/blob/main/.github/workflows/README.md). This is a simplistic way to get started with your first few tests.

1. Navigate to the Actions tab in the trestle-workspace
2. Click "Run workflow"
3. Make sure the branch: main and then click the green box "Run Workflow."

#### Annotated File Tree 📂

The annotated file tree can be referenced in [annotated-tree.md](https://github.com/hbraswelrh/creme-brulee/blob/95c4bbb99cbb0e0b28b459f19950983ca13d34b7/docs/annotated-tree.md) which shows the current state of the trestle-workspace public template. The annotations are consistent with the table below. 

**Example:** 

You want to create an OSCAL Catalog, OSCAL Profile, and an OSCAL Component Definition for a RHEL9 CIS Benchmark. The [ComplianceAsCode/content](https://github.com/ComplianceAsCode/content) repository houses up-to-date security content that is leveraged for transformation to OSCAL Content. 

Starting with the OSCAL Catalog, you want to base it off of the CIS Benchmark for RHEL9. Let trestle-bot do the magic 🪄 in GitHub Actions.


| OSCAL Content Created in `trestle-workspace`                                                                                                                               |                                 ComplianceAsCode/content location                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------:|
| [OSCAL Catalog](https://github.com/hbraswelrh/my-trestle-workspace/blob/main/catalogs/cis_rhel9/catalog.json)                                                              |    [cis_rhel9](https://github.com/ComplianceAsCode/content/blob/master/controls/cis_rhel9.yml)     | 
| [OSCAL Profile](https://github.com/hbraswelrh/my-trestle-workspace/tree/main/profiles)                                                                                     | [cis](https://github.com/ComplianceAsCode/content/blob/master/products/rhel9/profiles/cis.profile) |
| [OSCAL Component Definition](https://github.com/hbraswelrh/my-trestle-workspace/blob/main/component-definitions/rhel9/rhel9-cis_rhel9-l1_server/component-definition.json) | [cis](https://github.com/ComplianceAsCode/content/blob/master/products/rhel9/profiles/cis.profile) |

- The `annotated-tree.md`indicates "Control File cis_rhel9 from ComplianceAsCode/content" and "RHEL9 Profile for cis_rhel9-l1_x/12_x." Reference the second column of the table for how the elements of the ComplianceAsCode/content repository align with the trestle-workspace content.
- 
- The base file tree without annotations can be found in [workspace-tree.md](https://github.com/hbraswelrh/creme-brulee/blob/520790e4c8b261cfe2b83a804c1c2728bdacb3ef/docs/workspace-tree.md)

<!--The ComplianceAsCode/content repository is used for syncing and transforming content to OSCAL format.--> 

#### Workspace file tree 

```bash
.
├── assessment-plans
├── assessment-results
├── catalogs
│   └── cis_rhel9
│       └── catalog.json
├── component-definitions
│   └── rhel9
│       └── rhel9-cis_rhel9-l1_server
│           └── component-definition.json
├── markdown
│   ├── assessment-plans
│   ├── assessment-results
│   ├── catalogs
│   ├── component-definitions
│   ├── plan-of-action-and-milestones
│   ├── profiles
│   └── system-security-plans
├── plan-of-action-and-milestones
├── profiles
│   ├── rhel9-cis_rhel9-l1_server
│   │   └── profile.json
│   ├── rhel9-cis_rhel9-l1_workstation
│   │   └── profile.json
│   ├── rhel9-cis_rhel9-l2_server
│   │   └── profile.json
│   └── rhel9-cis_rhel9-l2_workstation
│       └── profile.json
└── system-security-plans


```

<!--_the one-stop shop for housing your catalogs, profiles, and component-definitions_ -->

<!-- GitHub-styled notifications can be used outside ordered lists. Available options are: NOTE, IMPORTANT, WARNING, TIP, CAUTION -->
<!--[!NOTE]
> (Important note or additional information relevant to this section)
 -->

(replace-me: Optional theory or background information relevant to this step)


<!-- IDEA: CREATE A REPO W A BUNCH OF ERRORS ON DOCS AND THEN GET PRS IN THAT REPO TO HAVE AN AUTOMATED WORKFLOW FOR CONTRIBUTIONS!!!!! -->

1. (replace-me: Additional instructions as needed)

<details>
<summary>Having trouble? 🤷</summary><br/>

- Reference the trestle-bot [`README.md`](https://github.com/complytime/trestle-bot/blob/main/README.md).
- [The guide for navigating public templates](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)
- (replace-me: Additional troubleshooting tips as needed)

</details>
