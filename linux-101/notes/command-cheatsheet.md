# Commands Cheatsheet (20+ Perintah Wajib)

## Navigasi & File
1. `pwd` - path kerja saat ini
2. `ls -lah` - list detail, termasuk file tersembunyi.
3. `cd`, `cd -` -  pindah direktori & toggle.
4. `mkdir -p demo/{logs,tmp}` - buat struktur bersarang.
5. `touch demo/README.txt` - buat file kosong.
6. `cp -r demo demo_copy ` - salin rekursif.
7. `mv demo_copy tmp_demo` - pindah/rename.
8. `rm -rf tmp_demo` - hapus direktori (hati-hati).

## Text Processing & Log
9. `cat /etc/os-release | head -5`
10. `grep -R "ssh" /etc`
11. `sed -n '1,10p' /var/log/syslog` *(atau log lain)*
12. `awk '{print $1}' /etc/passwd | sort | uniq | wc -l`

## Pencarian & Ukuran
13. `find /var/log -type f -size +10M` — cari file besar.

## Permission & Ownership
14. `chmod 640 demo/README.txt`
15. `sudo chown $USER:$USER demo/README.txt`
- Tambahan: `umask`, SUID/SGID (catatan di `notes/permissions-users.md`).

## Proses & Service
16. `ps aux | grep sshd` + `kill -9 <pid>`
17. `sudo systemctl status ssh` / `sudo systemctl enable --now ssh`
18. `journalctl -u ssh --since "today"`

## Jaringan
19. `curl -I https://example.com` — cek header/health.
20. `ssh -i ~/.ssh/id_ed25519 user@server` — key-based login.

---

### Tips “Kapan Dipakai”
- `journalctl -u <svc>` untuk root cause analisa service gagal start.
- `ss -tulpn` (tambahan) melihat port listening saat debug konektivitas.
- `find ... -size` untuk hunting disk full.