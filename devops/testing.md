# Here are some Pytest framework test example for simple code

    import pytest

    @pytest.fixture
    @pytest.mark.skip

    def numbers():
        a = 10
        b = 20
        c = 25
        return [a, b ,c]
    @pytest.mark.skip
    def test_method1 (numbers):
        x = 15
        assert numbers [0] == x
    @pytest.mark.skip

    def test_method2(numbers):
        y = 20
        assert numbers[1] == y
    @pytest.mark.skip
    def test_method3(numbers):
        z = 25 
        assert numbers[2] == z


API testing

    import pytest
    import requests
    import json



    def main_url():
        return "https://reqres.in/"

    def test_valid_login():
        url = "https://reqres.in/api/login/"
        data = {'email': 'eve.holt@reqres.in', 'password': 'cityslicka'}
        response = requests.post(url, data=data)
        token = json.loads(response.text)
        assert response.status_code == 200
        assert token["token"] == "QpwL5tke4Pnpja7X4"


    @pytest.fixture
    def test_rng():
        rng = rng_test_app

        with rng.test_client() as client:
            response = requests.get('/')
        

pytest marks tool

    import pytest

    def func(x):
            return x + 5

    @pytest.mark.one     
    def test_worker():
        assert func(3) == 8

    @pytest.mark.two
    def test_2():
            a = 18
            b = 20
            assert a+2 == b
