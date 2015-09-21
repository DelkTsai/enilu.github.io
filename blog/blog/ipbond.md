##Bonding��ģʽһ����7�֣�
    #defineBOND_MODE_ROUNDROBIN       0   ��balance-rrģʽ�������ĸ��ؾ���ģʽ
    #defineBOND_MODE_ACTIVEBACKUP     1   ��active-backupģʽ���������ݴ�ģʽ
    #defineBOND_MODE_XOR              2   ��balance-xorģʽ����Ҫ������֧��
    #defineBOND_MODE_BROADCAST        3    ��broadcastģʽ��
    #defineBOND_MODE_8023AD           4   ��IEEE 802.3ad��̬��·�ۺ�ģʽ����Ҫ������֧��
    #defineBOND_MODE_TLB              5   ����Ӧ���为�ؾ���ģʽ
    #defineBOND_MODE_ALB              6   �������⻯��ʽ
 
##bondingģ������й���ģʽ���Է�Ϊ���ࣺ
 �����͹���ģʽ�������͹���ģʽ��
 balance-rr ��broadcast���ڶ����͹���ģʽ��active-backup���������͹���ģʽ����balance-xor������Ӧ���为�ؾ���ģʽ��balance-tlb��������Ӧ���ؾ���ģʽ��balance-alb��Ҳ���ڶ����͹���ģʽ��IEEE 802.3ad��̬��·�ۺ�ģʽ��802.3ad�����������͹���ģʽ��

##��ϸ������7��ģʽ��
- 1��balance-rr ��mode=0��

    ��ת��Round-robin�����ԣ���ͷ��β˳�����ÿһ��slave�ӿ����淢�����ݰ�����ģʽ�ṩ���ؾ�����ݴ��������

- 2��active-backup��mode=1��

    �-���ݣ����������ԣ��ڰ��У�ֻ��һ��slave��������ҽ������slave�ӿ�ʧ��ʱ�Żἤ������slave��Ϊ�˱��⽻�����������Ҵ�ʱ�󶨵�MAC��ַֻ��һ���ⲿ�˿��Ͽɼ�����bongding��2.6.2�����Ժ�İ汾�У�����ģʽ�·���һ�ι���Ǩ��ʱ��bonding�����¼����slave�ϻ���һ�����߶��gratuitous ARP��bonding����salve�ӿ����Լ������ڽӿ��ϵ�����VLAN�ӿڶ��ᷢ��gratuitous ARP��ֻҪ��Щ�ӿ�������������һ��IP��ַ��VLAN�ӿ��Ϸ��͵ĵ�gratuitous ARP���ḽ���ʵ���VLAN id����ģʽ�ṩ�ݴ�������primary option��documented below��Ӱ�챾ģʽ����Ϊ��

- 3��balance-xor��mode=2��

    XOR���ԣ�������ѡ��Ĵ���hash���ԡ���ģʽ�ṩ���ؾ�����ݴ��������
- 4��broadcast��mode=3��

    �㲥���ԣ������е�slave�ӿ��ϴ������еı��ġ���ģʽ�ṩ�ݴ�������
- 5��802.3ad��mode=4��

    IEEE 802.3ad ��̬��·�ۺϡ�����������ͬ�����ʺ�˫��ģʽ�ľۺ��顣�ܸ���802.3ad�淶�������е�slave�������ۺ���·��Salve�ĳ�վѡ��ȡ���ڴ����hash���ԣ�Ĭ�ϲ����Ǽ򵥵�XOR���ԣ���hash���������ͨxmit_hash_policyѡ����Ըı䡣��Ҫע����ǣ��������еĴ�����Զ���802.3ad���ݣ�������802.3ad��׼��43.2.4�½��й��� packet mis-orderingҪ��ĵط�����ͬ�����ʵ���������ֺܴ�Ĳ����ݡ�
�Ⱦ�������
    - 1. ÿ��slave�Ļ�������֧��Ehtool��ȡ���ʺ�˫��״̬��
    - 2.������֧��IEEE 802.3ad��̬��·�ۺϡ�������Ľ���������Ҫʹ��ĳ�����÷�ʽ������802.3adģʽ��

- 6��balance-tlb��mode=5��
    
    ����Ӧ���为�ؾ��⣺�ŵ��󶨲���Ҫ����Ľ�����֧�֡����������ķֲ�ȡ���ڵ�ǰÿ��slave�ĸ��أ���������ٶȣ������������ӵ�ǰ��slave�Ľ��ա��������salve����������slave�ӹ�ʧ�ܵ�slave��MAC��ַ�������ա�
  �Ⱦ�������
  ÿ��slave�Ļ�������֧��Ehtool��ȡ����״̬��

- 7��balance-alb��mode=6��
    
    ����Ӧ���ؾ��⣺
        ����balance-tlb��ģʽ5���Լ�����IPV4�����Ľ��ո��ؾ��⣬���Ҳ���Ҫ����Ľ�����֧�֡����ո��ؾ���ͨ��ARPЭ��ʵ�֡�bonding���������ر���������ARP Replies��ARP��Ӧ���ģ���������bond��ĳһ��slave��Ӳ����ַ��дARP���ĵ�Դ��ַ��ʹ�ñ��������Բ�ͬ���豸ʹ�ò�ͬ��Ӳ����ַ�������������������ӵĽ�������Ҳ�Ǹ��ؾ���ġ�����������ARP Requestʱ��bonding����ͨ��ARP���ĸ��Ʋ�����ڵ��IP��Ϣ�����������ڵ���յ�ARP Reply��bonding������ȡ�ڵ��Ӳ����ַ���һ��Ӧһ�������󶨺õ�slave��Ӳ����ַ��ARP Reply�����͵Ľڵ㡣��ARPЭ�̵ĸ��ؾ������һ��������ÿ����bond��Ӳ����ַ�㲥ARP���ģ���ô�����ڵ㷢�͵�����ȫ��������һ��slave�ϣ�����ARP���¸��������нڵ��ʱ��ÿ���ڵ������ѧϰӲ����ַ�������������·��䡣���¼���һ��slave����һ���Ǽ����slave���¼����ʱ��Ҳ�ᵼ�½����������·��䡣�������������Ǵ��У���ת���ķ�����bond��һ��������ߵ�slave�ϡ���һ����·��������һ���µ�slave�����ʱ��bond�����³�ʼ��ARP Replies�����еĿͻ��ˡ�updelay������ֵ������ڻ��ߴ��ڽ�������forwarding delay������ARP Replies��������������
    �Ⱦ�������

    - 1.ÿ��slave�Ļ�������֧��Ehtool��ȡ����״̬��
    - 2. ��������֧�ֵ��豸��ʱ��������Ӳ����ַ��ҲҪ��ÿһ��slave����Ψһ��Ӳ����ַ�����curr_active_slaveʧ�ܣ�����Ӳ����ַ����ѡ�ϵ�curr_active_slaveӲ����ַ���滻��

##��CentOS������Bond0��Bond1��
����Ҫ��linux�Ƿ�֧��bonding,RHEL4�Ѿ�Ĭ��֧����.(�󲿷ַ��а涼֧��)
    # modinfo bonding
    filename:       /lib/modules/2.6.18-8.el5/kernel/drivers/net/bonding/bonding.ko
    author:         Thomas Davis, tadavis@lbl.gov and many others
    de.ion:    Ethernet Channel Bonding Driver, v3.0.3
    version:        3.0.3
    license:        GPL
    srcversion:     2547D22885C2FDF28EF7D98
����������������Ϣ���,˵���Ѿ�֧����.
###1������Bond 0 ���ؾ���
�ص�:

- ˫����ͬʱ����,ʵ�ָ��ؾ���,ĳһ����������ʱ,�������������ж�.
- �ָ�������������������ʱ,�����������жϼ���,Ȼ��˫����ͬʱ����.
- �༭��������ӿ������ļ�,ָ������IP 
        cp /etc/sysconfig/network-scripts/ifcfg-lo ifcfg-bond0
        vi  ifcfg-bond0
        DEVICE=bond0
        IPADDR=10.10.10.1
        NETMASK=255.255.255.0
        NETWORK=10.10.10.0
        BROADCAST=10.10.10.255
        ONBOOT=yes
        BOOTPROTO=none
        USERCTL=no
        GATEWAY=192.168.0.1

###2.��bond0���������,��ȷ��Ĭ��·���޹���
    [root@Linux ~]# route
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    10.0.0.0        *               255.255.255.0   U     0      0        0 bond0
    10.0.0.0        *               255.255.255.0   U     0      0        0 eth0
    10.0.0.0        *               255.255.255.0   U     0      0        0 eth1
    169.254.0.0     *               255.255.0.0     U     0      0        0 bond0
    default         10.0.0.1        0.0.0.0         UG    0      0        0 bond0

- �༭���������ļ���
        vi  ifcfg-eth0
        DEVICE=eth0
        BOOTPROTO=none
        ONBOOT=yes
        USERCTL=no
        MASTER=bond0
        SLAVE=yes

-  �༭����һ�����������ļ���
        vi  ifcfg-eth1
        DEVICE=eth1
        BOOTPROTO=none
        ONBOOT=yes
        USERCTL=no
        MASTER=bond0
        SLAVE=yes

###3���༭/etc/modprobe.conf �ļ���
��������һ�����ݣ���ʹϵͳ������ʱ����bondingģ�飬������������ӿ��豸Ϊ bond0 
������������ 

    alias bond0 bonding 
    options bond0 miimon=100 mode=0 
˵����
miimon������������·���ġ� ����:miimon=100����ôϵͳÿ100ms���һ����·����״̬�������һ����·��ͨ��ת����һ����·��
mode��ֵ��ʾ����ģʽ��������0��1,2,3����ģʽ�����õ�Ϊ0,1���֡�
   mode=0��ʾload balancing (round-robin)Ϊ���ؾ��ⷽʽ������������������
   mode=1��ʾfault-tolerance (active-backup)�ṩ���๦�ܣ�������ʽ�������Ĺ�����ʽ,Ҳ����˵Ĭ�������ֻ��һ����������,��һ��������. 
4 # vi /etc/rc.d/rc.local 
������������ 
### �����ȱ�ģʽ��,eht0 eth1�����Ĺ���˳��.
ifenslave bond0 eth0 eth1 
����ʱ�Ѿ��������������������.
�����ῴ��������Ϣ�ͱ�ʾ���óɹ���

    ................ 
    Bringing up interface bond0 OK 
    Bringing up interface eth0 OK 
    Bringing up interface eth1 OK

## ����Bond 1 �ȱ�ģʽ

�ص�:

- 1. ���ڹ�����������������,�л�����������,��ʱ���м伸����
- 2. �ָ�������������ʱ,�������������ж�.
��������һ�£�ֻ�ڵ�3���裬��mode���ó�1����.

            alias bond0 bonding 
            options bond0 miimon=100 mode=1