import os
import shutil

# Define file type categories and their respective extensions
file_types = {
    "Images": [".jpg", ".jpeg", ".png", ".gif", ".bmp", ".tiff"],
    "Documents": [".pdf", ".docx", ".txt", ".xlsx", ".pptx"],
    "Videos": [".mp4", ".mkv", ".avi", ".mov"],
    "Music": [".mp3", ".wav", ".aac"],
    "Archives": [".zip", ".tar", ".gz", ".rar"],
    "Other": []  # For other file types
}

# Function to create folders for each category
def create_folders(base_path):
    for category in file_types:
        folder_path = os.path.join(base_path, category)
        if not os.path.exists(folder_path):
            os.makedirs(folder_path)

# Function to move files based on their extension
def organize_files(base_path):
    # Loop through all files in the directory
    for filename in os.listdir(base_path):
        file_path = os.path.join(base_path, filename)
        
        # Skip directories
        if os.path.isdir(file_path):
            continue
        
        # Check the file extension and move to corresponding folder
        moved = False
        for category, extensions in file_types.items():
            if any(filename.lower().endswith(ext) for ext in extensions):
                # Create the folder if it doesn't exist
                folder_path = os.path.join(base_path, category)
                shutil.move(file_path, os.path.join(folder_path, filename))
                moved = True
                print(f"Moved {filename} to {category}")
                break
        
        # If the file doesn't match any known type, move it to "Other"
        if not moved:
            shutil.move(file_path, os.path.join(base_path, "Other", filename))
            print(f"Moved {filename} to Other")

# Main function to run the script
def main():
    base_path = input("Enter the path of the directory to organize: ").strip()
    
    if not os.path.exists(base_path):
        print(f"Error: The directory '{base_path}' does not exist.")
        return
    
    create_folders(base_path)
    organize_files(base_path)
    print("Files have been organized.")

# Run the script
if __name__ == "__main__":
    main()
