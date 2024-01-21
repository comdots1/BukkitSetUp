@ -0,0 +1,76 @@
import os
import platform
import requests


def download_file(url, destination):
    response = requests.get(url)
    with open(destination, 'wb') as file:
        file.write(response.content)


def generate_script(file_path, min_ram, max_ram):
    current_os = platform.system()

    script_extension = '.bat' if current_os == 'Windows' else '.sh'

    script_content = f"""\
    java -Xms{min_ram} -Xmx{max_ram} -jar {file_path} --nogui
    """

    script_file_path = f"server/start{script_extension}"
    with open(script_file_path, 'w') as script_file:
        script_file.write(script_content)

    print(f"{script_file_path} 스크립트가 저장되었습니다.")


def update_eula(eula_file_path):
    user_agreement = input("End User License Agreement(EULA)에 동의하십니까? ('true' 또는 'false'를 입력하십시오.): ").lower()

    if user_agreement == 'true':
        with open(eula_file_path, 'w') as eula_file:
            eula_file.write("eula=true")
        print(f"{eula_file_path}가 저장되었습니다.")
    else:
        print("End User License Agreement(EULA)에 동의하셔야 서버를 만드실 수 있습니다.")
        exit()


def run_script(script_file_path):
    execute_script = input("서버를 바로 실행하시겠습니까?('yes' 또는 'no'를 입력하십시오.): ").lower()

    if execute_script == 'yes':
        current_os = platform.system()

        os.chdir('server')

        if current_os == 'Windows':
            os.system(f'start {script_file_path}')
        else:
            os.system(f'bash {script_file_path}')
    else:
        print("서버를 실행하지 않겠습니다. 스크립트와 bukkit 파일은 server 폴더에 저장되었습니다.")


if __name__ == "__main__":
    file_url = 'https://api.papermc.io/v2/projects/paper/versions/1.20.4/builds/393/downloads/paper-1.20.4-393.jar'
    file_name = 'bukkit.jar'

    eula_path = 'server/eula.txt'

    min_ram = input("최소 램 사용량을 입력하세요. 뒤 단위를 꼭 M 또는 G로 입력하십시오. (ex: 512M): ")
    max_ram = input("최대 램 사용량을 입력하세요. 뒤 단위를 꼭 M 또는 G로 입력하십시오. (ex: 2G): ")

    if not os.path.exists('server'):
        os.makedirs('server')

    print("잠시만 기다리십시오. 버킷 파일을 다운로드 중입니다...")
    download_file(file_url, f'server/{file_name}')
    print("다운로드가 완료되었습니다.")

    update_eula(eula_path)

    generate_script(file_name, min_ram, max_ram)

    run_script("start.bat" if platform.system() == 'Windows' else "start.sh")
