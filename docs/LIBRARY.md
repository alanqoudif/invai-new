# File Library with Privacy Controls

The Enhanced File Library feature provides comprehensive file management capabilities with advanced privacy controls, allowing users to organize and secure their documents effectively.

## Features

### 🔒 Privacy Controls
- **Public Files**: Accessible to all users in the system
- **Private Files**: Only accessible to the file owner
- **Granular Access Control**: Fine-tuned permissions per file

### 📁 File Management
- **Upload Interface**: Drag-and-drop or click to upload files
- **Visibility Toggle**: Set files as public or private during upload
- **Filter System**: View all files, only public files, or only private files
- **File Metadata**: Display file size, type, and privacy status

### 🎨 User Interface
- **Modern Design**: Clean, intuitive interface with Arabic language support
- **Responsive Layout**: Works seamlessly across desktop and mobile devices
- **Visual Indicators**: Clear privacy status indicators for each file

## Configuration

### Environment Variables

Add these environment variables to your `.env` file or Docker configuration:

```bash
# Enable file privacy features
ENABLE_FILE_PRIVACY=true

# Set default visibility for new files (public/private)
DEFAULT_FILE_VISIBILITY=private

# Enable the library interface
LIBRARY_ENABLED=true
```

### Docker Configuration

The Dockerfile and docker-compose.yaml have been updated to include these environment variables by default.

## Usage

### Accessing the Library
1. Navigate to the main application
2. Click the "Library" button in the navigation bar (folder icon)
3. You'll be taken to `/workspace/library`

### Uploading Files
1. In the Library interface, click "Upload new file"
2. Select your file using the file picker or drag and drop
3. Choose visibility (Public/Private) using the toggle
4. Click "Upload" to save the file

### Managing Files
1. Use the filter buttons to view:
   - **All**: Show all your files
   - **Public**: Show only public files
   - **Private**: Show only private files
2. Each file displays:
   - File name
   - File size
   - Content type
   - Privacy status (عام for public, خاص for private)

### File Access Control
- **Private files**: Only visible to the owner
- **Public files**: Visible to all users
- **API filtering**: Backend automatically filters files based on user permissions

## Technical Implementation

### Frontend Changes
- New `Library.svelte` component with privacy controls
- Updated `Navbar.svelte` with Library button
- Modified `uploadFile` API function to support access control
- Reactive filtering system for file visibility

### Backend Changes
- Updated `/files/` API endpoint to support `access_control` parameter
- Modified file listing to filter based on user permissions
- Enhanced file creation with privacy metadata

### Database Schema
Files now include an `access_control` field with the following structure:
```json
{
  "read": ["user_id"] // Array of user IDs with read access
}
```

## Security Considerations

- Files are filtered at the API level to prevent unauthorized access
- User authentication is required for all file operations
- Privacy settings are enforced both in the UI and backend
- File ownership is tracked and validated

## Troubleshooting

### Files Not Showing
- Check that `LIBRARY_ENABLED=true` in your environment
- Verify user authentication
- Ensure proper file permissions

### Privacy Settings Not Working
- Confirm `ENABLE_FILE_PRIVACY=true` is set
- Check that the backend has been updated with the new API changes
- Verify database schema includes `access_control` field

### Upload Issues
- Check file size limits
- Verify proper file permissions
- Ensure the uploads directory is writable

## Future Enhancements

- Group-based file sharing
- Advanced permission levels (read, write, delete)
- File versioning with privacy inheritance
- Bulk privacy operations
- Integration with external storage providers