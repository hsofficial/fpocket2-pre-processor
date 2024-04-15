#!/bin/bash
# original author:kangsgo (https://kangsgo.cn/p/%E8%87%AA%E5%86%99focket2-%E4%B8%ADmd%E5%A4%84%E7%90%86%E8%84%9A%E6%9C%AC/)
# KR trans and optimizw : dlwlrma@pusan.ac.kr
# name: fpocket2 MD preprosessing script
#
# method:
# 이 스크립트는 gromacs를 통한 MD 산출물을 fpocket2에서 처리할 수 있도록 변환합니다.
# 기본 궤적 파일 이름은 md_1_nojump.xtc, 기본 tpr은 md_1.tpr입니다.
#
# 필요에 따라 다음 매개변수를 수정하여 사용하십시오.
#
# 총 궤적 수(ps)
# numtray=150000
# 간격(예시 : 150프레임마다) 추출
# dttray=1000

#init
echo "=== fpocket2 MD data preprosessing script ==="
echo "author:kangsgo, KR trans and optimize : HS Na (dlwlrma@pusan.ac.kr)"

# md_1_nojump_fit.xtc 생성이 필요할 경우 아래 2줄의 코드를 주석 해제하여 사용 
# echo "> md_1_nojump_fit.xtc 파일을 생성합니다."
# echo -e "1\n1\n"|gmx trjconv -f md_1_nojump.xtc -s md_1.tpr -fit rot+trans -o md_1_nojump_fit.xtc

#디렉토리 마이그레이션 생성
echo "> fpocket2 폴더를 생성하고 필요한 파일을 복사합니다."
mkdir fpocket2
cp md_1_nojump_fit.xtc fpocket2/md_1_nojump_fit.xtc
cp md_1.tpr fpocket2/md_1.tpr

echo "> fpocket2 폴더로 이동합니다."
cd fpocket2

#txt 파일 생성
echo "> txt 파일을 생성합니다."
touch mdpocket_input.txt

#총 궤적 수(ps) [기본값 200000ps=200ns]
numtray=200000
#간격마다 추출(기본 세팅은 1ns마다 추출파일을 생성합니다.) [기본값 1000]
dttray=1000

#궤적 변수를 설정합니다.
numdttray=$[$numtray/$dttray]
echo "> 트랙간격은 $numdttray 입니다."
count=0
num=0
while [ $num -le $numtray ]
do
	echo "> 값은 $num 입니다."
	echo "> 카운트 값은 $count 입니다."
	count=$[ $count +1]
	num=$[$dttray*$count]
	
	#매개변수를 ns로 변경하고 1000으로 나눕니다.
	ns=$[$num/1000]
	echo -e "1\n"|gmx trjconv -f md_1_nojump_fit.xtc -s md_1.tpr -dump $num -o snap_$ns.pdb
	
	#파일 쓰기를 진행합니다.
	echo "snap_$ns.pdb\n">>mdpocket_input.txt
done

echo ">>> 작업이 성공적으로 완료되었습니다. 이제 해당 위치에서 mdpocket을 실행하세요. <<<"
