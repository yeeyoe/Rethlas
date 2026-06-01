第一步：启动验证服务

cd agents/verification
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn api.server:app --host 0.0.0.0 --port 8091  

第二步：运行生成代理

cd agents/generation
python3 -m venv .venv
source .venv/bin/activate
pip install -r mcp/requirements.txt
./tests/run.sh

运行自定义数学问题
cd agents/generation
source .venv/bin/activate
PROBLEM_FILE=data/my_problem.md ./tests/run.sh
PROBLEM_FILE=data/sum/sum.md ./tests/run.sh
PROBLEM_FILE=data/modrep/modrep.md ./tests/run.sh

查看证明结果
cd agents/generation
./site/serve.sh