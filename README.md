# 🧩 Guide GitLab and Jira Integration

This guide explains how to connect **Jira** and **GitLab** so that commits, merge requests, and issue updates in GitLab automatically sync with Jira issues.

---

## ⚙️ Jira Configuration

### 1. Log in as Administrator
Log into Jira using an account with **Administrator** privileges.

### 2. Access User Management
Click the **⚙️ (gear icon)** in the top-right corner → select **User Management**.

### 3. Verify Authentication
If prompted, re-enter the Administrator password.

### 4. Create a GitLab Integration User
Click **Create User**, then:
- **Username:** `gitlab`
- **Password:** (choose a secure password and note it down)

This user allows GitLab to update Jira issues automatically.

### 5. Create a Group for GitLab
From the sidebar, click **Groups**, then:
- Enter `gitlab-developers` and click **Add Group**

The new group should now appear in the list.

### 6. Add the GitLab User to the Group
Click **Edit members** beside `gitlab-developers`, then:
- In **Add members to selected groups**, enter `gitlab`
- Click **Add selected users**

Now the `gitlab` user is a member of `gitlab-developers`.

### 7. Grant Permissions to the Group
To allow GitLab to comment and update issues:

1. Go to **⚙️ → Issues**.  
2. From the left sidebar, select **Permission Schemes**.  
3. Click **Add Permission Scheme**, give it a name, and click **Add**.  
4. Click **Permissions** next to your new scheme.  
5. In the **Administer Projects** section, click **Edit**.  
6. Choose **Group** under *Granted to*, select `gitlab-developers`, and click **Grant**.

Now, the `gitlab-developers` group has write permissions on all Jira projects.

---

## 🧰 GitLab Configuration

### 1. Log in as Administrator
Log into GitLab using an **Administrator** account.

### 2. Open Admin Area
Click the **🔧 (wrench icon)** in the top bar → select **Admin Area**.

### 3. Enable Jira Service
From the left sidebar, go to **Service Templates**, then select **Jira**.

### 4. Activate the Integration
- Click **Active** to enable Jira service.  
- Make sure **Commit** and **Merge Request** options are also checked.

### 5. Configure Connection Details
Fill in the following fields:
- **Web URL:** Your Jira base URL (e.g., `http://jira.school.ai`)
- **Jira API URL:** Leave this blank.
- **Username:** `gitlab`
- **Password:** Password you created for the Jira `gitlab` user.

### 6. Add Transition IDs
Transition IDs are used to change issue statuses (e.g., *To Do → In Progress → Done*).

To find them, open this URL in your browser:

```
https://<your-jira-domain>/rest/api/2/issue/DRIVER-232/transitions
```

Replace:
- `<your-jira-domain>` with your Jira URL  
- `DRIVER-232` with an existing issue key (that is *Open*)

Copy the Transition IDs from the JSON response and enter them separated by commas in **Transition IDs**.

### 7. Save Settings
Click **Save** to complete the connection.

Now GitLab is successfully linked with Jira!  
You should see a **Jira** link under the **Issues** section of your GitLab project sidebar.

### 8. (Optional) Disable GitLab’s Built-in Issue Tracker
If you prefer using only Jira:
- Go to your project → **Settings → General**
- Under **Visibility, project features, permissions**, disable **Issues**

---

## 🔄 Smart Commits (Jira Automation from GitLab)

**Smart Commits** allow you to interact with Jira issues directly from GitLab commit messages.

### General Format:
```
<ignored text> <ISSUE_KEY> <ignored text> #<COMMAND> <optional COMMAND_ARGUMENTS>
```

### Examples:

#### 1. Add a Comment
```
JRA-34 #comment develop favorite places feature.
```
→ Adds a comment to issue **JRA-34**.

#### 2. Log Work
```
JRA-34 #time 1d 2h 30m develop favorite places feature.
```
→ Adds a **worklog** of 1 day, 2 hours, and 30 minutes to **JRA-34**, with a comment.

#### 3. Comment, Log Time, and Resolve Issue
```
JRA-34 #comment develop favorite places feature. #time 2h 5m #resolve
```
→ Adds a comment, logs 2h 5m of work, and changes the issue’s status to **Resolved**.

---

## ✅ Summary

| Step | Tool | Action |
|------|------|--------|
| 1 | Jira | Create `gitlab` user |
| 2 | Jira | Add user to `gitlab-developers` group |
| 3 | Jira | Grant write permissions to group |
| 4 | GitLab | Enable Jira service |
| 5 | GitLab | Configure connection using Jira credentials |
| 6 | GitLab | Add Transition IDs |
| 7 | GitLab | Use Smart Commits to control Jira from commits |

---

### 💡 Example Integration Flow
1. Developer commits to GitLab with message:  
   `APP-21 #comment fixed login bug #time 30m #resolve`
2. Jira automatically:
   - Adds a comment *“fixed login bug”*
   - Logs *30 minutes* of work
   - Marks **APP-21** as **Resolved**

---

**Enjoy seamless integration between GitLab and Jira! 🚀**
