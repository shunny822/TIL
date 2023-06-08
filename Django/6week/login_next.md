# 로그인 후 특정 페이지 이동

### 특정 페이지에 접근하기 위해 로그인이 필요한 상황

1. next query string
    ```python
    # app/views.py

    # 그룹 참가
    def group_join(request, group_pk):
        group = Group.objects.get(pk=group_pk)

        if not request.user.is_authenticated:
            return redirect(f'/accounts/login/?next={request.path}')
        
        ...
    ```
    - redirect 시 로그인 url('/accounts/login/') 뒤에 next라는 query string 작성

    - `next=<로그인 후 이동할 url>`

    - 여기서는 로그인 후 다시 그룹 참가로 돌아와야 하므로 현재경로인 'request.path'를 작성함

2. template에 next의 value값을 받을 input tag 작성
    ```python
    # login template
    ...

    <form action="{% url 'accounts:login' %}" method="POST">
      {% csrf_token %}
      {% for field in form  %}
      <div>
          {{ field }}
      </div>
      {% endfor %}
      {% if request.GET.next %}
        <input type="hidden" name="next" value="{{ request.GET.next }}">
      {% endif %}
      <button class='login-btn'>로그인</button>
    </form>

    ...
    ```
    - login form에 type이 hidden인 input tag를 작성

    - name을 지정하고 value값으로 next query string의 값을 입력


3. accounts의 로그인 함수에서 next 처리
    ```python
    # accounts/views.py

    def login(request):
        if request.method == 'POST':
            form = CustomAuthenticationForm(request, request.POST)
            if form.is_valid():
                auth_login(request, form.get_user())
                next_url = request.POST.get('next')
                if next_url:
                    return redirect(next_url)
            return redirect('groups:index')
          
        ...
    ```
    - login 후 POST method로 request가 넘어왔을 때 get method를 이용해 next의 value를 받아올 수 있음

    - next의 값이 있는 경우 해당 url로 redirect하면 원했던 그룹참가 url로 이동함