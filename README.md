
# Cài đặt và sử dụng WSL2 + ZSH + Windows Terminal để làm việc với môi trường linux trên windows 10/ 11.

## Chúng ta muốn gì?
Về cơ bản, chúng ta muốn một môi trường làm việc giống với trên Macbook hoặc máy tính chạy linux của mình nhưng ở trên windows.
  - Một terminal có thể cài đặt và custom như **iTerm**
  - ZSH shell with [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)
  - [Powerlevel10k](https://github.com/romkatv/powerlevel10k) theme 
  - Hỗ trợ Docker và sử dụng tốt với VScode

## Vậy bạn có thể có gì sau khi setup?
Một cửa sổ terminal ngầu lòi như thế này trên windows.
![Terminal][terminal-image]

## Để cài đặt và sử dụng, chúng ta cần:
  - Một máy tính chạy Windows 11/ 10 (prefer Windows 11)
  - Tối thiểu 8GB RAM (prefer 16GB trở lên)
  - Đã enable virtualization trong BIOS setting

## Đầu tiên, chúng ta sẽ cài đặt WSL2
- Chúng ta cần làm theo các [bước](https://docs.microsoft.com/en-us/windows/wsl/install-win10) này của Microsoft.
- Đây là một vài bước mà bạn cần làm:

  Mở PowerShell với quyền Admin và chạy những command sau:
  ```
  # Install WSL
  dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

  # Enable Virtual Machine Platform
  dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
  ```
  Sau 2 câu lệnh trên thì chúng ta cần chuyển đổi version mặc định của wsl sang version 2, để sử dụng wsl2:
  ```
  # Use WSL2 per default
  wsl --set-default-version 2
  ```

## Bước 2, chúng ta sẽ cài đặt 1 Linux Distribution và Windows Terminal
- Hãy mở Microsoft Store và cài đặt Distribution mà bạn muốn, cùng với đó là Windows Terminal. Tớ sẽ cài Ubuntu 20.04 và Windows Terminal.
- Sau khi cài đặt Ubuntu 20.04, mở nó lên thì nó sẽ yêu cầu bạn đặt username và password cho nó.

## Bước 3: chúng ta sẽ setup Windows Terminal
  - Mở Windows Terminal lên và ấn tổ hợp phím `Ctrl + ,`, bạn sẽ thấy giao diện Setting của Windows Terminal. Ở góc dưới bên trái có nút **Open JSON File**, hãy ấn vào đó, bạn sẽ có thể chỉnh sửa setting trên 1 Text Editor.
  - Vì mình muốn sử dụng Ubuntu 20.04 là profile chính, mặc định mở lên khi mình mở Windows Terminal nên mình sẽ set **defaultProfile** là **guid** của profile Ubuntu 20.04
  - Để cái Terminal của chúng ta nhìn xịn sò hơn mặc định, mình sẽ tìm một cái Color Scheme đẹp và sử dụng nó, bạn có thể tham khảo một số Scheme được tạo bởi Microsoft tại [đây](https://docs.microsoft.com/en-US/windows/terminal/customize-settings/color-schemes) hoặc bởi cộng đồng tại [đây](https://windowsterminalthemes.dev/)
  - Color Scheme mà mình dùng ở ảnh bên trên là *MaterialOcean*, mình chỉ cần set giá trị này cho trường **colorScheme** là xong. 
  - Mình muốn con trỏ ở terminal là dạng 1 filledBox nên sẽ set **cursorShape** trong profile Ubuntu là ***filledBox***
  - Minh muốn khi mở Windows Terminal lên thì sẽ truy cập luôn vào thư mục ***~*** nên mình sẽ set **commandline** của profile là ***wsl.exe ~***
  - Cuối cùng thì mình cần phải chọn font phù hợp để hỗ trợ tốt nhất cho Powerlevel10k, được [recommend](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k) là *MesloLGS NF*. Sau khi cài đặt Font thì các bạn hãy chỉnh **font.face** là ***MesloLGS NF***, thế là xong. Lưu ý, font thì cần được cài trên Windows để Windows Terminal và VScode có thể sử dụng được.
  - Ngoài ra các bạn có thể chỉnh sửa thêm nhiều setting khác như là hình nền, độ transparent bằng cách set trực tiếp  trên giao diện setting của Windows Terminal hoặc thay vào các attribute tương ứng.
  - Sau cùng thì profile setting của Ubuntu có thể trông như sau:
  ```
  {
    "acrylicOpacity": 0.71999999999999997,
    "backgroundImage": "C:\\Users\\HikariQ\\Desktop\\rubik-wpp.jpg",
    "backgroundImageOpacity": 0.28000000000000003,
    "colorScheme": "MaterialOcean",
    "commandline": "wsl.exe ~",
    "cursorShape": "filledBox",
    "font": 
    {
        "face": "MesloLGS NF",
        "size": 14
    },
    "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
    "hidden": false,
    "name": "Ubuntu-20.04",
    "source": "Windows.Terminal.Wsl",
    "useAcrylic": true
  }
  ```

## Bước 4: Cài đặt oh-my-zsh
  Chạy những command sau:
  ```
  sudo apt install fonts-powerline
  sudo apt install fonts-font-awesome
  sudo apt install zsh
  sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```
  Sau khi chạy xong thì bạn có thể restart Windows Terminal để start 1 session mới.

## Bước 5: Cài đặt một số plugin cần thiết cho ZSH
  - [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
  - [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
  - [zsh-completions](https://github.com/zsh-users/zsh-completions)
  ```
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
  git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
  ```
  Sau khi cài đặt xong, hay mở file *~/.zshrc* và chỉnh sửa một số thứ sau: 
  ```
  # Use Powerlevel10k theme
  ZSH_THEME="powerlevel10k/powerlevel10k"
  
  # Use plugins
  plugins=(
          git
          docker
          zsh-autosuggestions
          zsh-completions
          zsh-syntax-highlighting
  )
  ```
  Sau khi thực hiện chỉnh sửa xong, thì khi quay lại cửa sổ terminal, Bộ setting của Powerlevel10k sẽ hiện ra và yêu cầu bạn thực hiện một vài cài đặt, nếu nó không hiện ra bạn có thể gõ lệnh *p10k configure* để nó hiện ra. Cần thêm thông tin về [oh-my-zsh-plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins) và [Powerlevel10k](https://github.com/romkatv/powerlevel10k#configuration) thì bạn có thể tìm hiểu thêm.

## Bước 6: cài đặt Docker lên WSL2.
  Việc cài đặt docker lên WSL2 Ubuntu 20.04 không khác gì cài đặt lên 1 máy tính chạy Ubuntu 20.04 cả, bạn chỉ việc làm theo các bước ở [đây](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04).
  Nhớ thực hiện đủ các bước từ cài đặt tới việc thêm user của bạn vào group *docker* để có thể chạy lệnh docker mà không cần sudo. Sau khi cài đặt xong bạn chỉ việc khởi động lại Terminal là có thể sử dụng. Tuy nhiên mỗi khi bạn tăt máy tính của mình đi và bật lại, thì bạn cần start docker service trước khi sử dụng bằng câu lệnh *sudo service docker start*

## Bước 7: cài đặt VScode
  Chúng ta cần thêm vài setting cho VSCode để intergrated terminal trong vscode có thể hiển thị đẹp y như Windows Terminal. Bạn hãy ấn tổ hợp phím *Ctrl + ,* để mở cửa sổ setting của VSCode
  - Chúng ta cần set font cho terminal: hãy tìm kiếm *terminal.integrated.fontFamily* và set giá trị là ***'MesloLGS NF', monospace, PowerlineSymbols*
  - Để có thể set Color Scheme cho VSCode terminal, bạn có thể chỉnh sửa các attributes trong **workbench.colorCustomizations**, có sẵn rất nhiều Color Scheme ở [đây](https://glitchbone.github.io/vscode-base16-term/#/harmonic-dark), chọn cái mà mình thích và  triển thôi.



[terminal-image]: images/terminal.jpg