require 'rake'

namespace :es do
  desc "Run Elastalert"
  task :run do
    sh 'docker run -d -p 3030:3030 \
-v `pwd`/config/elastalert.yaml:/opt/elastalert/config.yaml \
-v `pwd`/config/config.json:/opt/elastalert-server/config/config.json \
-v `pwd`/rules:/opt/elastalert/rules_templates \
--name elastalert \
 bitsensor/elastalert:latest'
  end
# --net="host" \
# -v `pwd`/rule_templates:/opt/elastalert/rule_templates \

  desc "Get log"
  task :log do
    sh 'docker logs elastalert'
  end

  desc "tailf log"
  task :tailf do
    sh 'docker logs elastalert -f'
  end

  desc "Show ps"
  task :show do
	sh 'docker container ps'
	sh 'docker container ps -a'
  end

  desc "Remove container"
  task :remove do
	sh 'docker container stop elastalert'
	sh 'docker container rm elastalert'
    Rake::Task["es:show"].invoke()
  end

  desc "login container"
  task :login do
    sh 'docker exec -it elastalert sh'
  end

  desc "restart container"
  task :restart do
    Rake::Task["es:remove"].invoke()
    Rake::Task["es:run"].invoke()
    Rake::Task["es:show"].invoke()
  end
end

namespace :ansible do
  desc "syntax check"
  task :syntax do
    sh 'ansible-playbook main.yml --syntax-check'
  end

  desc "build check"
  task :check do
    sh 'ansible-playbook main.yml -i inventory -vv --check'
  end

  desc "build"
  task :build do
    sh 'ansible-playbook main.yml -i inventory -vv'
  end

  desc "install requirements from galaxy"
  task :install do
    # sh 'sudo yum install ansible -y'
    sh 'sudo pip install ansible ansible-lint'
    # sh 'ansible-galaxy install -r requirements.yml'
  end
end

namespace :ci do
  desc "Run CI test"
  task :default do
    Rake::Task["ansible:build"].invoke()
    Rake::Task["inspec:default"].invoke()
  end
end
