如果使用Windows Defender防火墙，则可能还需添加两条命令
（通常GFE会自动给Denfender添加规则，如果连不上，尝试关闭Denfender以确认是否是它的问题）
以管理员身份打开命令提示符或PowerShell窗口
运行以下2个命令：
netsh advfirewall firewall add rule name="GameStream UDP" dir=in protocol=udp localport=5353,47998-48010 action=allow
netsh advfirewall firewall add rule name="GameStream TCP" dir=in protocol=tcp localport=47984,47989,48010 action=allow


如果你想删除这两个规则请分别输入以下命令

netsh advfirewall firewall delete rule name="GameStream UDP"
netsh advfirewall firewall delete rule name="GameStream TCP"