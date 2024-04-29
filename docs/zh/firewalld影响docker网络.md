\# 开启 NAT 转发 firewall-cmd --permanent --zone=public --add-masquerade

\# 检查是否允许 NAT 转发 firewall-cmd --query-masquerade

\# 禁止防火墙 NAT 转发 firewall-cmd --remove-masquerad