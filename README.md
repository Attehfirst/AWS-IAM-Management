# AWS-IAM-Management

## AWS IAM Manager Script – Documentation

## 🧠 Purpose

This script is designed to automate the process of managing AWS IAM (Identity and Access Management) resources via the AWS CLI. It reflects foundational DevOps practices by applying shell scripting techniques to manage:

* IAM users
* IAM groups
* Permissions via managed policies

---

## 🎯 Objectives

The script fulfills the following objectives:

1. **Define an array of IAM usernames**.
2. **Create IAM users from that array using AWS CLI**.
3. **Create a group named `admin` and attach AWS-managed `AdministratorAccess` policy to it**.
4. **Add all defined users to the `admin` group**.
5. **Provide robust logging and feedback throughout the process**.

---

## 🏗️ Script Design & Thought Process

### 1. **Variable Definition**

I started by defining an array of usernames for whom I want to create IAM accounts. This makes it easy to loop over them and avoid repetitive code:

```bash
IAM_USER_NAMES=("Tom" "Mary" "Jack" "Jones" "Rex")
```

Arrays make the script scalable and easy to update. You only need to modify this one line if you want to change the set of users.

---

### 2. **Function: `create_iam_users()`**

**Purpose**: To create IAM users by looping through the array.

**Key Steps**:

* Loop through each user
* Use `aws iam create-user` to attempt user creation
* Log whether creation succeeded or failed

**Error Handling**: Redirect output and check return codes to account for failures (e.g., if a user already exists).

---

### 3. **Function: `create_admin_group()`**

**Purpose**: To create a group named `admin` (if it doesn't already exist) and attach a policy.

**Key Steps**:

* Use `aws iam get-group` to check if the group exists.
* If not, create the group with `aws iam create-group`.
* Attach AWS's managed `AdministratorAccess` policy using:

  ```bash
  aws iam attach-group-policy --group-name admin \
       --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
  ```

**Error Handling**:

* Commands are checked with `$?` to ensure they succeed.
* Output is suppressed where needed to keep the script clean.

---

### 4. **Function: `add_users_to_admin_group()`**

**Purpose**: To add each user to the `admin` group.

**Key Steps**:

* Iterate through the same array of users
* Use `aws iam add-user-to-group` for each
* Provide feedback on each operation

**Scalability**: You can reuse the same array logic here, which improves consistency and reduces code duplication.

---

### 5. **Main Function Structure**

The script wraps execution in a `main()` function to make the code clean and modular.

It starts by checking whether `aws` CLI is installed:

```bash
if ! command -v aws &> /dev/null; then
```

This ensures the user is set up correctly before running any AWS operations.

---

## 📋 Pre-requisites

Before running the script, ensure:

1. **AWS CLI is installed** on your machine or EC2 instance.
2. **AWS CLI is configured** with an IAM user/role that has permissions to manage IAM (i.e., create users, groups, and attach policies).
3. Your terminal environment supports Bash and AWS CLI commands (e.g., WSL, EC2, Ubuntu, etc.).

---

## 💪 How to Use

### 1. Save Script

Save the script to a file:

```bash
vim aws-iam-manager.sh
```

Paste the script contents and save.

### 2. Make it Executable

```bash
chmod +x aws-iam-manager.sh
```

### 3. Run the Script

```bash
./aws-iam-manager.sh
```

---

## 🔪 Testing & Validation

You can verify successful execution by:

* Running `aws iam list-users`
* Checking `aws iam get-group --group-name admin`
* Verifying group membership via:

  ```bash
  aws iam get-group --group-name admin
  ```

---

## 🔐 Security Note

This script uses **AdministratorAccess** policy for demo purposes. In a real-world scenario, **least privilege** should be followed—assign only necessary permissions.

## ✅ Conclusion

This script demonstrates how DevOps engineers can use simple Bash scripting and AWS CLI to automate IAM management in a scalable and repeatable way.

---

## 🟎️ Screenshots

[media pointer="file-service://file-5CxfK1kt9P48EnMkpJVkC1"]
[media pointer="file-service://file-8H6rGmvPuh174adajzkRZv"]


---



