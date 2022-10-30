# zkapp

zkapp with snarkjs like iden3

### 1. 설치!

npm 으로 snarkjs 모듈을 설치한다.

`npm install -g snarkjs@latest`

그리고 snarkjs 의 `groth16 prove`, `g16p` --help 옵션을 확인한 후

a power of tau ceremony 를 이용하는 명령어를 입력한다.
사실 나는 그게 뭔지 잘 모르겠다. `bn128` 과 `bls12-381` 를 지원한다는데...!?
숫자 12는 constraints 수를 2의 최대 12승까지로 설정한다는 것이다. 지원하는 최대 설정값은 28이다.

`snarkjs powersoftau new bn128 12 pot12_0000.ptau -v`
=> pot12_0000.ptau 생성

### 2. 엔트로피 설정

`snarkjs powersoftau contribute pot12_0000.ptau pot12_0001.ptau --name="First contribution" -v`
=> pot12_0001.ptau 생성

위 명령어를 입력하면 내가 설정할 수 있는 랜덤 시드 같은 문구를 입력하는 창이 나오는데
나는 그냥 내 정체성 키워드를 입력했다! 뭔지는 시크릿임.

ptau 는 history 파일이라고 생각하면 쉬울 것 같다.

### 3. Second Contribution

`snarkjs powersoftau contribute pot12_0001.ptau pot12_0002.ptau --name="Second contribution" -v -e="some random text"`
=> pot12_0002.ptau 생성

### 4. 써드파티 프로그램을 이용해서 third contribution 을 생성한다.

1. `snarkjs powersoftau export challenge pot12_0002.ptau challenge_0003`
   => challenge_0003 생성
2. `snarkjs powersoftau challenge contribute bn128 challenge_0003 response_0003 -e="some random text"`
   => response_0003 생성
3. `snarkjs powersoftau import response pot12_0002.ptau response_0003 pot12_0003.ptau -n="Third contribution name"`
   => pot12_0003.ptau 생성

### 5. 지금까지 생성한 contribution verify!

MPC(multi-party computation)

`snarkjs powersoftau verify pot12_0003.ptau`
=> contribution 들 검증

### 6. random beacon

`snarkjs powersoftau beacon pot12_0003.ptau pot12_beacon.ptau 0102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f 10 -n="Final Beacon"`
=> pot12_beacon.ptau 생성

### 7. phase 2 준비?!

`snarkjs powersoftau prepare phase2 pot12_beacon.ptau pot12_final.ptau -v`
=> tauG1 => tauG2 => alphaTauG1 => betaTauG1 순으로 DEBUG
