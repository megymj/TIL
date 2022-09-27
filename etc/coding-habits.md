1. 자바에서 외부 라이브러리(.jar) import 할 때, gradle이나 maven으로 묶은 뒤 dependency 추가하는 방식으로 사용하기

2. GitHub Action에서, access-key-id 등 private한 정보는 직접 작성해서 push하면 안된다.
```yaml
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@master
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-2
```

3. GitHub에 디렉토리 경로 (e.g Data/benchmark/... ) 등과 같은 것은 올리지 않도록 주의. 협업시에 커밋 기록이 남아버리므로 지우는데 상당한 노력이 소모된다.
