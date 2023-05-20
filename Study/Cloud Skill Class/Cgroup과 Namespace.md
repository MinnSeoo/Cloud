# **Cgroup과 Namespace 정리**

---

## **Cgroup이란?**

Cgroup은 Control groups의 약자로 프로세스들이 사용할 수 있는 컴퓨팅의 자원을 제한 및 격리 시킬 수 있는 리눅스 커널 기능이다.

Cgroup을 이용하면 Memory, CPU, Network, Device, l/O 같은 자원들을 제한 시킬 수 있다.

## **Namespace란?**

Namespace는 한 프로세스 집합이 다른 프로세스의 리소스를 보고 다른 프로세스 집합이 다른 리소스 집합을 볼 수 있도록 커널 리소스를 분활하는 Linux 커널의 기능이다.

→ 즉, Namespace의 주요기능은 프로세스를 **격리**한다는 것이다.

예를 들어 하나의 PID(프로세스 ID)를 Namespace를 통해 격리를 시킨다면
하나의 시스템에서 동일한 PID가 2개인것 처럼 프로세스를 만들 수 있다.
(실제로 2개가 존재하진 않지만 그렇게 보임.)