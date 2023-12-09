# Writeup level02



In found in this level02 a pcap file :



![ls](https://cdn.discordapp.com/attachments/1164485225875783701/1165281765863206943/image.png?ex=65464836&is=6533d336&hm=33200cf38e087a18b57a59e09f8f4acd2d478ced1691cb58e12501c56355f62c&)

the owner of this file is flag02 that mean this file is packet capture of the network 
let donwload this file we can open server to download it or use scp command :


![scap](https://cdn.discordapp.com/attachments/1164485225875783701/1165297169616601168/image.png?ex=6546568f&is=6533e18f&hm=adecc2e71fd750ee7b958e1d7a1c887d5da7ca7730d344a81132c74e38e5ffe5&)

We use wireshark tool to read that file and follow tcp to see any readable data:

![scap](https://cdn.discordapp.com/attachments/1164485225875783701/1165298731801915443/image.png?ex=65465803&is=6533e303&hm=d0663dfd76a984fa9fcd827b75f123d4659e958ea83bdbf2345a3d3f042e4b1f&)


The capture caught the user attempting to login to wwwbugs. The login was incorrec.

let try to read this byte of that password from hex not ascii

we get :
```
000000B9  66                                                 f
000000BA  74                                                 t
000000BB  5f                                                 _
000000BC  77                                                 w
000000BD  61                                                 a
000000BE  6e                                                 n
000000BF  64                                                 d
000000C0  72                                                 r
000000C1  7f                                                 .
000000C2  7f                                                 .
000000C3  7f                                                 .
000000C4  4e                                                 N
000000C5  44                                                 D
000000C6  52                                                 R
000000C7  65                                                 e
000000C8  6c                                                 l
000000C9  7f                                                 .
000000CA  4c                                                 L
000000CB  30                                                 0
000000CC  4c                                                 L
000000CD  0d                                                 .
```
if we check ascii table and in Hexadecimal Representation we can 7f not equal to '.' that but to DEL(127) that mean that user try to delete charatcher that non-printable character. that mean characters after 7f should be deleted and last character also noT dot('.').

that mean the result is ft_waNDReL0L
