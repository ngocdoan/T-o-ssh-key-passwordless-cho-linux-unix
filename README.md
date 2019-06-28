# Đăng nhập ssh trên máy server
ssh user@ip.address -p portnumber
# Tạo ssh-key passwordless cho Linux/Unix trên server
ssh-keygen -t rsa -b 2048
Máy sẽ tạo ra 2 file:
- id_rsa.pub: public key (ổ khóa)
- id_rsa: private key (chìa khóa)
# Thêm ổ khóa vào danh sách được ủy quyền
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
# Phân quyền
chmod 600 ~/.ssh/authorized_keys
# Sửa file cấu hình trên server
sudo vi /etc/ssh/sshd_config
uncomment PubkeyAuthentication yes
:wq! để lưu lại
# Restart sshd trên mac OS
sudo launchctl unload /System/Library/LaunchDaemons/ssh.plist
sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist
# Tải file private key về máy client
scp -p portnumber user@ip.address:/server/patch/id_rsa /client/patch/
# Thực hiện đăng nhập bằng key đã tải về
ssh -i /client/patch/id_rsa user@ip.address -p portnumber
---------------------------------------------------------------
# C1: nếu muốn máy client đăng nhập mà ko đường dẫn key nữa thì phải import vào keychain trên máy client (khi restart phải chạy lại list lệnh này)
# Chạy import agent ssh-key
eval `ssh-agent -s`
# Import ssh-key
ssh-add -K /path/your-private-key
# Giờ chỉ việc chạy lệnh ssh user@ip.address -p portnumber, máy sẽ ko hỏi gì mà đăng nhập luôn
# C2: Tạo file khai báo như sau trên máy client (khi restart vẫn chạy phà phà)
Host test1
	HostName ip.address
	Port portnumber
	User username
	PreferredAuthentications publickey
	IdentityFile ~/.ssh/privatekey
	IdentitiesOnly=yes
 # Từ giờ chỉ cần gõ ssh test1 là xong.
