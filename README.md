# homework_3

1. 객체 추적(Object Tracking)
정의: 영상에서 특정 객체(차량 등)를 여러 프레임에 걸쳐 동일하게 식별하고 위치를 추적하는 기술.

단계:

검출(Detection): 각 프레임에서 객체 위치를 탐지 (예: Bounding Box)

연결(Association): 시간적으로 연속된 프레임 사이에서 동일 객체인지 판별

대표 기법 (직접 구현 시):

Centroid Tracking (중심 좌표 추적)

Bounding box의 중심점을 기준으로 거리 기반으로 ID 부여

유클리디안 거리(Euclidean Distance)를 기준으로 프레임 간 비교

2. 픽셀 좌표 → 실제 거리 변환 (Camera Calibration)
정의: 영상 속 픽셀 단위의 거리(예: 50px)를 실제 거리 단위(예: 1m)로 변환하기 위한 과정

기법:
Homography: 평면에서의 변환 행렬 (4점 이상의 대응점 필요)

Scale Calibration: 기준 길이(예: 차선 너비, 횡단보도 폭 등)를 실제 측정값과 비교해 px당 거리 추정

EX):
실제 거리 3m에 해당하는 길이가 영상에서는 150px이면, 변환 비율은
1m = 50px → 1px = 0.02m

3. 속도 계산
: 이동거리/시간

EX):
vehicle_data = {
    1: {
        'positions': [(x1, y1), (x2, y2), ...],
        'speeds': [v1, v2, ...],
        'directions': [angle1, angle2, ...]
    },
    2: {
        ...
    }
}


​
 
4. 방향 계산 (Heading)
정의: 벡터의 방향을 각도로 변환

EX):
import math

def calculate_angle(p1, p2):
    dx, dy = p2[0] - p1[0], p2[1] - p1[1]
    angle = math.degrees(math.atan2(-dy, dx))  # y축 반전 고려
    return angle % 360



5. 데이터 구조 설계

EX):
class VehicleTrack:
    def __init__(self, object_id):
        self.id = object_id
        self.positions = []  # [(x, y, timestamp)]
        self.speeds = []     # [속도값]
        self.directions = [] # [각도 또는 벡터]

    def update(self, x, y, timestamp):
        self.positions.append((x, y, timestamp))
        # 이전 위치와 비교하여 속도 및 방향 계산
