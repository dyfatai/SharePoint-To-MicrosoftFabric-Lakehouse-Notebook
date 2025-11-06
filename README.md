# SharePoint to Fabric Lakehouse ETL

Automated notebook to download Excel files from SharePoint and load them into Microsoft Fabric Lakehouse with overwrite capability.

## Consideration
This Notebook requires the sharing link to be set to anyone can edit. Effort is being put into managing authentication within the notebook.

## Overview

This PySpark notebook automates the process of:
1. Downloading Excel files from SharePoint using sharing links
2. Writing files to Microsoft Fabric Lakehouse
3. Overwriting existing files on each run
4. Providing detailed logging and error handling

## Use Case

Ideal for organizations that need to:
- Regularly sync Excel files from SharePoint to Fabric Lakehouse
- Maintain up-to-date data in their lakehouse
- Avoid manual file uploads
- Schedule automated data refreshes

## Prerequisites

- Microsoft Fabric workspace with a Lakehouse
- SharePoint site with Excel files
- Permissions to create sharing links for SharePoint files
- Fabric notebook environment

## Setup Instructions

### Step 1: Create Sharing Links

For each Excel file you want to sync:
1. Navigate to the file in SharePoint
2. Right-click the file → **Share**
3. Select **"Anyone with the link can edit"**
4. Click **Copy link**
5. Save this link for configuration

> **Note:** If "Anyone with link" is disabled by your organization, you'll need to work with your IT admin to set up alternative authentication (App Registration).

### Step 2: Configure the Notebook

Open **Cell 3** (Configuration) and update:
```python
# Update your lakehouse path
lakehouse_abfs_path = "abfss://YOUR_WORKSPACE@onelake.dfs.fabric.microsoft.com/YOUR_LAKEHOUSE.Lakehouse/Files/YOUR_FOLDER"

# Update SharePoint details
sharepoint_tenant = "your-tenant"
sharepoint_site = "your-site"

# Add your files
source_files = [
    {
        "url": "ORIGINAL_SHAREPOINT_URL",
        "sharing_link": "PASTE_SHARING_LINK_HERE",
        "lakehouse_name": "desired_filename.xlsx",
        "description": "File description"
    }
]
```

### Step 3: Run the Notebook

1. Upload the notebook to your Fabric workspace
2. Attach it to your Lakehouse
3. Run all cells sequentially
4. Check the output for success/failure status

## 📊 Configuration Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `lakehouse_abfs_path` | ABFS path to your lakehouse folder | `abfss://workspace@onelake.dfs.fabric.microsoft.com/lakehouse.Lakehouse/Files/data` |
| `sharepoint_tenant` | Your SharePoint tenant name | `contoso` |
| `sharepoint_site` | SharePoint site name | `projects` |
| `url` | Original SharePoint file URL | Full URL from browser |
| `sharing_link` | SharePoint sharing link | Link copied from Share dialog |
| `lakehouse_name` | Desired filename in lakehouse | `report_2024.xlsx` |
| `description` | Human-readable description | `Monthly Sales Report` |

## Scheduling

To run this notebook automatically:

1. Create a **Fabric Pipeline**
2. Add a **Notebook activity**
3. Select this notebook
4. Configure a **Schedule trigger** (hourly, daily, etc.)
5. Save and activate the pipeline

## Features

- ✅ **Automatic overwrite** - Updates files on each run
- ✅ **Error handling** - Continues processing even if one file fails
- ✅ **Detailed logging** - Shows progress and status for each file
- ✅ **Batch processing** - Handles multiple files in one run
- ✅ **File verification** - Validates Excel file format before writing
- ✅ **Summary report** - Provides success/failure counts

## Troubleshooting

### Download Failed (403 Error)
**Cause:** Sharing link permissions are insufficient  
**Solution:** Ensure "Anyone with link can edit" is enabled

### Download Failed (Status 404)
**Cause:** File not found or sharing link expired  
**Solution:** Regenerate the sharing link

### Write Failed
**Cause:** Insufficient permissions to lakehouse  
**Solution:** Verify workspace access and lakehouse permissions

### No Sharing Link Configured
**Cause:** Empty `sharing_link` field  
**Solution:** Add sharing link in Cell 3 configuration

## 📂 File Structure
```
SharePoint-To-MicrosoftFabric-Lakehouse-Notebook/
│
├── README.md                          # This file
├── SharePoint_to_Lakehouse_ETL.ipynb  # Main notebook
└── examples/
    └── sample_config.py               # Sample configuration
```

## 🔒 Security Considerations

- Sharing links with "Anyone can edit" provide public access
- Consider using time-limited links or more restrictive sharing
- For production, use App Registration with certificate-based auth
- Never commit sharing links to public repositories
- Use Fabric secrets/Key Vault for sensitive configuration

## 🤝 Contributing

Contributions are welcome to make this notebook work without setting the SharePoint URL 'to anyone can edit'! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request with clear description 

## 📄 License

MIT License - Feel free to use and modify

## 👤 Author

[Fatai Sanni]  
[dyfatai]

## 🙏 Acknowledgments

Built for Microsoft Fabric and SharePoint integration

---

**⭐ If this helps you, please star the repository!**
```
