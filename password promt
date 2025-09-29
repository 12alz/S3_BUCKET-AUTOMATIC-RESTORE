#!/bin/bash

# ==========================
# CONFIGURATION
# ==========================
BUCKET="bakup-db-test"
MOUNT_POINT="$HOME/.s3_buckets"
PASSFILE="$HOME/.passwd-s3fs"
SECRET="qweqwe"
REGION_URL="https://s3.ap-northeast-1.amazonaws.com"

mkdir -p "$MOUNT_POINT"

# ==========================
# CHECK IF ALREADY MOUNTED
# ==========================
if mountpoint -q "$MOUNT_POINT"; then
    fusermount -u "$MOUNT_POINT" 2>/dev/null || umount "$MOUNT_POINT" 2>/dev/null
    zenity --info --text="S3 bucket disconnected."
else
 
    PASSWORD=$(zenity --password --title="S3 Drive Unlock")
    if [ "$PASSWORD" != "$SECRET" ]; then
        zenity --error --text="Wrong password!"
        exit 1
    fi

   
    if s3fs "$BUCKET" "$MOUNT_POINT" \
        -o passwd_file="$PASSFILE" \
        -o url="$REGION_URL" \
        -o use_path_request_style \
        -o nonempty >/dev/null 2>&1; then
        
       
        dolphin "$MOUNT_POINT"
        
      
        fusermount -u "$MOUNT_POINT" 2>/dev/null || umount "$MOUNT_POINT"
        zenity --info --text="S3 bucket disconnected."
    else
        zenity --error --text="Failed to connect S3 bucket!"
    fi
fi
