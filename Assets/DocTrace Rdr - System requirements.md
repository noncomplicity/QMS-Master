Process: [[Requirements Management]]
Requirements: [[EN 62304 5.2 Software requirements analysis]]
Type: #Specification 
Object: [[Doctrace Rdr]]
## 1. Purpose

The purpose of the QMS Frontend Web Portal is to provide a secure, user-friendly interface for interacting with the Quality Management System (QMS). The portal shall allow authenticated users to access organizational documentation in a controlled, read-only manner.

The portal serves as the presentation layer of a Git-based QMS architecture, integrating with backend services and repositories that store and version-control QMS documentation and configuration.

## 2. Intended Users

**Super Administrator** – Manages organizations, system configuration, and global user accounts.

**Organization Administrator** – Manages members, role assignments, repositories, and organizational settings.

**Authenticated User** – Reads documentation. (No editing or contribution rights are available in MVP.)

## 3. System Overview

The system consists of the following major components:

- **Frontend Web Application** providing the user interface for login, administration, and documentation viewing.
    
- **Backend Service/API** handling authentication, authorization, user and organization data, and serving documentation metadata and content.
    
- **Repository Integration Layer** connecting to Git-based repositories to retrieve published documentation artifacts.
    
- **Database** storing organizations, users, roles, and audit logs.
    
- **Static Documentation Storage** for built documentation artifacts per organization and release.
    

## 5. Functional Requirements

### 5.1 Organization Management

**FR-1:** The system shall allow Super Administrators to create, suspend, and delete organizations.

**FR-2:** The system shall allow Organization Administrators to view and manage organization-level settings (name, repositories, documentation settings).

**FR-3:** Each user shall belong to one or more organizations through a membership record.

### 5.2 User and Role Management

**FR-4:** The system shall implement role-based access control with the following roles: Super Administrator, Organization Administrator, and Authenticated User.

**FR-5:** Each membership shall define the user’s role within the organization.

**FR-6:** Organization Administrators shall be able to invite users to join their organization.

**FR-7:** The system shall enforce access controls based on user roles.

**FR-8:** All administrative actions shall be logged with timestamp, user, and affected entities.

### 5.3 Authentication and Authorization

**FR-9:** The system shall require authentication before granting access to any protected resource.

**FR-10:** The system shall implement SSO (Single Sign-On) with GitHub, GitLab, Google, and Microsoft (Entra ID/Azure AD).

**FR-11:** Each authenticated session shall include organization context and expire automatically after a defined period of inactivity.

**FR-12:** Authorization decisions shall be enforced server-side using the user’s organization role.

### 5.4 Administration

**FR-13:** Super Administrators shall access a global administration panel to manage organizations and monitor system health.

**FR-14:** Organization Administrators shall access an organization-level administration panel to manage users, roles, and repository connections.

**FR-15:** Administrative interfaces shall clearly display audit logs and role assignments.

### 5.5 Documentation Access

**FR-16:** The system shall provide access to controlled QMS documentation stored in Git-based repositories.

**FR-17:** The system shall retrieve and display documentation in a human-readable format (HTML or Markdown-rendered).

**FR-18:** Each documentation view shall display metadata including Document ID (From CI), version (from git/CI), build timestamp (from git/CI), Process (from source), Requirements (from source), **Owner (from source)**, and Type (from source).

**FR-19:** The system shall provide full-text search across documentation within an organization.

### Reader Navigation & Interaction (within Documentation Access)

_(User stories for this section are maintained in [[DocTrace Rdr - Navigation Requirements]])_

### 5.6 Audit Logging

**FR-20:** The system shall maintain an immutable audit log for all administrative and role-related actions.

**FR-21:** Audit log entries shall contain timestamp, user ID, organization ID, action type, and affected entity.

## 6. Non-Functional Requirements

### 6.1 Security

**NFR-1:** All communication shall use TLS encryption (HTTPS).

**NFR-2:** Authentication tokens shall be short-lived and signed using industry-standard algorithms.

**NFR-3:** Sensitive data such as passwords shall be hashed and never stored in plaintext.

**NFR-4:** Access control shall be enforced on every API call.

### 6.2 Performance

**NFR-5:** Documentation loading time shall not exceed 2 seconds under normal conditions.

### 6.3 Scalability

**NFR-6:** The system shall support multiple organizations with logically separated data.

**NFR-7:** The design shall allow scaling horizontally to handle multiple organizations concurrently.

### 6.4 Reliability

**NFR-8:** All critical operations (e.g., role changes, org configuration) shall be transactional.

### 6.5 Auditability

**NFR-9:** All role and access changes shall be recorded in a way that can be exported for audits.

**NFR-10:** Audit logs shall be retained for a minimum of 5 years.

### 6.6 Usability

**NFR-11:** The UI shall clearly indicate the user’s active organization and role.

**NFR-12:** Documentation pages shall include breadcrumbs (a simple navigation trail showing where the user is within the site, such as 'Home > Documentation > QMS Processes > Change Control') and search.

**NFR-13:** The design shall be responsive for desktop and mobile use.

## 7. Interfaces

Git Repository Interface used to pull released documentation artifacts; Authentication Interface for SSO integration; Frontend API (REST or GraphQL) for organization, user, role, and documentation management; and optional CI/CD Integration webhooks to notify the portal when documentation builds are completed.

### 7.1 CI/CD Process

**Purpose:** Define how documentation is built, versioned, tagged, and deployed to the portal to ensure traceability.

**Triggers:** On push to the release branch (e.g., `main`) and on creation of a version tag (e.g., `vX.Y.Z`). Manual re-run allowed by Org Admins.

**Build:** Static site build (e.g., MkDocs/Docusaurus) produces HTML and a searchable JSON index. Build captures commit SHA, branch, tag (if any), build timestamp, and pipeline ID.

**Versioning/Tagging:** If a tag exists, it becomes the display Version. Otherwise the short commit SHA is used. Pipelines may create a tag according to org rules.

**Environments:** `staging` (preview) and `production` (published). Only `production` artifacts are visible to Authenticated Users.

**Artifact Storage:** Built artifacts are stored per organization and release in object storage (e.g., bucket path `/org/<id>/releases/<tag-or-sha>/`).

**Deployment:** On successful production build, artifacts are uploaded and cache is invalidated. The portal ingests provenance (commit, branch, tag, build time) and updates the `Release` and `DocumentIndex` records.

**Traceability:** Every published page displays Document ID, Version (tag or SHA), Build timestamp, Process, Requirements, Owner, and Type (per FR-18). Audit logs record build and publish events.

## 8. Data Model (High-Level)

**Entities:**

`User(id, name, email, auth_provider, created_at)`

`Organization(id, name, status, created_at)`

`Membership(id, user_id, organization_id, role)`

`AuditLog(id, organization_id, user_id, action, target, timestamp)`

`DocumentIndex(id, organization_id, release_id, path, title, version)`

`Release(id, organization_id, branch, tag, commit_sha, status, created_at)`

## 10. Document Metadata

Frontmatter (YAML or JSON blocks at the top of each document) shall define the metadata fields for every controlled document, including Process, Owner, Type, and Object. This ensures consistent and machine-readable metadata extraction during builds.

All controlled documents include the following metadata fields:

Document ID, Version, Status, Owner, Process, Type, and Date of Last Approval.

## 9. Future Extensions (Out of MVP Scope)

- CAPA, risk, and document workflow management.
    
- Integrated approval processes and digital signatures.
    
- Automated traceability reports.
    
- Editing and version proposal directly from the portal.
    
- Integration with issue trackers (e.g., GitHub/GitLab issues).
    
- Commenting and feedback features.