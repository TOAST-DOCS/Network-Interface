## Network > Network Interface > 콘솔 사용 가이드


#### 네트워크 인터페이스 생성

* 이름: 네트워크 인터페이스의 이름을 입력합니다.

* VPC: 네트워크 인터페이스를 생성할 VPC를 선택합니다.

* 서브넷: 네트워크 인터페이스를 생성할 서브넷(subnet)을 선택합니다.

* 가상 IP: 이중화를 위한 가상 IP용으로 네트워크 인터페이스를 생성합니다.<br>가상 IP로 사용할 IP를 다른 인스턴스, 로드 밸런서 등의 리소스에 할당하지 못하도록 선점할 수 있습니다.<br>가상 IP로 생성된 네트워크 인터페이스는 "VIRTUAL_IP" 장치명을 가지며, 라우팅 테이블의 라우트 설정에서 라우트의 게이트웨이로 지정할 수 있습니다.<br>가상 IP로 생성한 네트워크 인터페이스는 인스턴스 생성 시 직접 연결하여 사용할 수 없습니다.

* 보안: 스푸핑을 방지하고 네트워크 인터페이스에 보안 그룹을 설정할 수 있는 기능입니다.<br>보안 기능을 사용하면 네트워크 인터페이스의 IP/MAC 주소를 출발지로 갖지 않는 패킷이 해당 네트워크 인터페이스를 통해 발신되는 것을 차단하여 스푸핑을 방지하고, 보안 그룹을 이용해 트래픽 제어를 할 수 있습니다. <br>보안 기능을 사용하지 않으면 이러한 스푸핑 방지 기능 및 보안 그룹 설정이 모두 해제됩니다. 이로 인해 모든 패킷이 허용되므로 자체적인 보안 기능(방화벽 등)을 갖추지 않은 경우 반드시 사용할 것을 권고합니다.

* 보안 그룹 선택: 네트워크 인터페이스에서 사용할 보안 그룹을 선택합니다. 여러 개 선택이 가능합니다.

**확인**을 클릭하면 네트워크 인터페이스가 생성됩니다.

> [참고] 인스턴스 생성 시 네트워크 인터페이스 생성을 하는 경우와 기존 네트워크 인터페이스 지정을 하는 경우 차이점
>
> * 신규로 네트워크 인터페이스를 생성하는 경우
>   해당 인스턴스를 삭제하면 자동으로 생성했던 네트워크 인터페이스도 함께 삭제가 됩니다.
> * 기존 네트워크 인터페이스를 지정해서 인스턴스를 생성하고, 연결된 인스턴스를 삭제하는 경우
>   해당 인스턴스에서 사용하였던 네트워크 인터페이스는 함께 삭제되지 않습니다. 남겨진 네트워크 인터페이스는 추후 다른 인스턴스에서 연결해서 사용이 가능합니다.


#### 네트워크 인터페이스 변경
네트워크 인터페이스의 속성 중 이름, IP, 보안 그룹을 변경할 수 있습니다.
플로팅 IP 연결이 없어야 변경이 가능합니다.
IP 변경이 반영되기 위해서는 인스턴스 재부팅을 하고 DHCP 갱신이 될 때까지 시간이 필요합니다.

#### 네트워크 인터페이스 삭제
선택한 네트워크 인터페이스를 삭제할 수 있습니다.
삭제하려면 네트워크 인터페이스가 장치에 연결되어 있지 않아야 합니다.

#### 소스/대상 확인 변경
소스/대상 확인을 활성화 또는 비활성화하여 인스턴스가 수신하는 트래픽의 소스 또는 대상인지 확인할 수 있습니다.
NAT 인스턴스의 네트워크 인터페이스인 경우에는 소스/대상 확인이 활성화되지 않아야 하므로, 이 메뉴를 통해 활성화/비활성화 기능을 관리할 수 있습니다.

#### 가상 IP 사용
가상 IP 용으로 생성한 네트워크 인터페이스는 IP 선점과 라우팅 테이블에서 라우트 게이트웨이로 지정하기 위한 용도이며, 특정 인스턴스와 직접 연결되지 않습니다.
가상 IP로 지정한 IP를 사용하기 위해서는 다음과 같은 절차가 필요합니다.

1. 보안 설정 변경
네트워크 인터페이스의 **보안** 기능은 스푸핑 방지 기능을 활성화하여, 인스턴스에서 출발하는 패킷이 클라우드 시스템에서 할당한 IP를 출발지 주소로 갖는 경우에만 전송될 수 있습니다.
그러므로 가상 IP를 사용하기 위해서는 이러한 스푸핑 방지 기능을 우회하도록 보안 설정 변경이 필요합니다.
    * 보안 기능 해제<br>
        네트워크 인터페이스의 **보안** 기능을 사용하지 않으면 스푸핑 방지 기능이 해제되어 가상 IP를 제한 없이 사용할 수 있습니다.<br>
        단, 이 경우 보안 그룹을 적용할 수 없으므로 자체적인 보안 기능을 갖춘 경우가 아니라면 권장하지 않습니다.
    * 추가 허용 주소 등록<br>
        가상 IP를 사용할 인스턴스들의 네트워크 인터페이스에 가상 IP를 '추가 허용 주소'로 등록하면, 이 IP를 출발지 주소로 갖는 패킷의 전송을 허용합니다.<br>
        제한적으로 스푸핑 방지 기능을 우회하고, 보안 기능을 해제하지 않아 보안 그룹 적용도 가능합니다.<br>
        다만 현재 콘솔에서 기능을 제공하고 있지 않기 때문에 해당 기능 사용을 위해서는 가상 IP 생성 후 아래 양식으로 [1:1 문의](https://www.ngsc.go.kr/kr/support/inquiry)를 해주셔야 합니다(추후 콘솔에 기능 제공 예정).
        <pre><code class="language-console">1. 추가 허용 주소 등록 신청 요청
   \- 조직 ID/프로젝트 ID:
   \- 리전: (퍼블릭/공공, KR1/KR2)<br>2. 가상 IP
   \- IP 주소:
   \- VPC ID:<br>3. 추가 허용 주소 등록 대상
   \- 인스턴스명: 
   \- 인스턴스 UUID:
   \- 가상 IP를 연결할 네트워크 인터페이스의 IP:</code></pre>   

2. 인스턴스 운영 체제 구성
보안 설정 변경이 완료되면 인스턴스에 가상 IP를 구성합니다. 클라우드 시스템에서는 이에 대한 기능을 별도로 제공하지 않으며, 인스턴스 내에서 인스턴스의 운영 체제나 배포판에 따른 적절한 방법으로 IP를 추가하거나, 애플리케이션을 이용하여 직접 구성해야 합니다.

> [참고] 가상 IP 사용 시 다음 사항에 주의하시기 바랍니다.
> * 가상 IP와 이를 사용할 인스턴스들의 네트워크 인터페이스는 같은 서브넷 내에 위치해야 합니다.
> * 가상 IP를 사용하여 통신할 때, 이를 사용하는 인스턴스는 기존 보안 그룹이 그대로 적용됩니다. 즉, 가상 IP에는 별도로 적용되는 보안 그룹 규칙이 없습니다.
> * 가상 IP를 출발지 주소로 갖는 패킷을 수신해야 하는 인스턴스는 보안 그룹에 가상 IP를 원격으로 수신할 수 있는 규칙을 확인해야 합니다.
>    가상 IP는 어느 보안 그룹에도 속하지 않으므로, 수신 규칙에 원격으로 '보안 그룹'을 지정해 둔 경우 가상 IP가 해당 보안 그룹에 포함되지 않아 통신이 되지 않을 수 있습니다.
> * 가상 IP를 사용하는 인스턴스는 네트워크 구성에 따라 인스턴스 내부 라우팅 규칙을 직접 조정해야 할 수 있습니다.