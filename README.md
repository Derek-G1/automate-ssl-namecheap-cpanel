# How to Automate Free SSL Certificates on Namecheap (cPanel)

**The Problem:** Standard free SSLs require manually copying keys into cPanel every 60 days.  
**The Solution:** We use a tool called `acme.sh` to issue the certificate and **automatically push it to cPanel**. It runs on a schedule, so you never have to touch it again.

### Prerequisites
* Login access to cPanel.
* The domain name you want to secure.
* A valid email address (for renewal notifications).

---

## Step 1: Open the Terminal
You do not need to install any software on your computer. You can do this directly from the browser.

1.  Log in to your **Namecheap cPanel**.
2.  Use the search bar at the top to find **"Terminal"**.
3.  Click the **Terminal** icon to open the black command screen.
    * *Note: If the screen doesn't load, go to "Exclusive for Namecheap Customers" > "Manage Shell" in cPanel and make sure SSH Access is toggled **ON**.*

---

## Step 2: Install the Automation Tool
Copy and paste the command below into the terminal.  
*(Replace `email@example.com` with your actual email)*.

```bash
curl [https://get.acme.sh](https://get.acme.sh) | sh -s email=email@example.com
```

**IMPORTANT:** Once that finishes, you **must** run this command to activate the tool:

```bash
source ~/.bashrc
```

---

## Step 3: Find Your Website Folder
We need to tell the tool where your website files live.

* **Main Domain:** Usually `/home/username/public_html`
* **Addon Domain:** Usually `/home/username/yourdomain.com`

If you aren't sure, run this command to list your folders:

```bash
ls -d /home/$USER/*
```

---

## Step 4: Issue the Certificate
Replace `yourdomain.com` with your website URL and `/path/to/folder` with the folder you found in Step 3.

```bash
acme.sh --issue -d yourdomain.com -d [www.yourdomain.com](https://www.yourdomain.com) -w /path/to/folder
```

* **Wait:** You should see a progress bar.
* **Success:** Look for green text saying **"Cert success"**.

---

## Step 5: Connect to cPanel (The "No Copy-Paste" Step)
This command links the certificate to the cPanel API so it updates automatically.  
*(Replace `yourdomain.com` with your actual domain)*.

```bash
acme.sh --deploy -d yourdomain.com --deploy-hook cpanel_uapi
```

* **Success:** Look for the message: `Successfully deployed certificate... via UAPI`.

---

## Verification
1.  Go to your website in a browser (e.g., `https://yourdomain.com`).
2.  Click the **Lock Icon** in the address bar.
3.  The certificate should show today's date.

**Done!** The system will now check every day and automatically renew and reinstall the certificate before it expires.
