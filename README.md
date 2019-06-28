# Tạo ssh-key passwordless cho Linux/Unix
ssh-keygen -t rsa -b 2048
# Máy sẽ tạo ra 2 file:
# - id_rsa.pub: public key (ổ khóa)
# - id_rsa: private key (chìa khóa)
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
# Phân quyền
chmod 600 ~/.ssh/authorized_keys
# Sửa file cấu hình
sudo vi /etc/ssh/sshd_config
# uncomment PubkeyAuthentication yes
# :wq! để lưu lại
# Restart sshd trên mac OS
sudo launchctl unload /System/Library/LaunchDaemons/ssh.plist
sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist
# Tải file private key về máy client
scp -p portnumber user@ip.address:/server/patch/id_rsa /client/patch/
# Thực hiện đăng nhập bằng key đã tải về
ssh -i /client/patch/id_rsa user@ip.address -p portnumber
