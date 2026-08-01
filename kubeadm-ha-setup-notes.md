Kubernetes High Availability (HA) using kubeadm
Project Architecture

                           kubectl
                              |
                              |
                      Virtual IP (VIP)
                      192.168.1.100:6443
                              |
                     -------------------
                     |                 |
                 HAProxy-1        HAProxy-2
                 (MASTER)          (BACKUP)
                     |                 |
                Keepalived      Keepalived
                     |
      -----------------------------------------
      |                  |                    |
  Master-1          Master-2            Master-3
(API Server)      (API Server)       (API Server)
 Scheduler          Scheduler          Scheduler
 Controller         Controller         Controller
 etcd               etcd              etcd
      |________________________________________|
               etcd Cluster Replication
                     |
          ----------------------------
          |                          |
      Worker-1                  Worker-2
