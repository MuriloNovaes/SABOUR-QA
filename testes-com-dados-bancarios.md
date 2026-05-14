
# Teste 1: Listar todos os bancos
try:
    response_banks = requests.get(f"{BASE}/banks/v1", timeout=10)
    test1_condition = response_banks.status_code == 200 and isinstance(response_banks.json(), list) and len(response_banks.json()) > 0
    run_test(
        "Listar todos os bancos",
        test1_condition,
        "Status 200 e lista não vazia recebida.",
        f"Falha: Status {response_banks.status_code} ou lista vazia/formato incorreto.",
        response_banks.json() if response_banks.status_code == 200 else None
    )
except Exception as e:
    run_test("Listar todos os bancos", False, "", f"Erro: {e}")

# Teste 2: Buscar banco pelo código (código 1)
try:
    response_bank_id = requests.get(f"{BASE}/banks/v1/1", timeout=10)
    test2_condition = response_bank_id.status_code == 200 and response_bank_id.json().get("code") == 1
    run_test(
        "Buscar banco pelo código (ID 1)",
        test2_condition,
        "Status 200 e banco com ID 1 encontrado.",
        f"Falha: Status {response_bank_id.status_code} ou ID do banco incorreto.",
        response_bank_id.json() if response_bank_id.status_code == 200 else None
    )
except Exception as e:
    run_test("Buscar banco pelo código (ID 1)", False, "", f"Erro: {e}")

# Teste 3: Banco inexistente
try:
    response_none = requests.get(f"{BASE}/banks/v1/99999", timeout=10)
    run_test(
        "Banco inexistente",
        response_none.status_code == 404,
        "Status 404 retornado corretamente.",
        f"Falha: Status {response_none.status_code} em vez de 404.",
        response_none.json() if response_none.status_code == 200 else None
    )
except Exception as e:
    run_test("Banco inexistente", False, "", f"Erro: {e}")

print("\n--- Testes Concluídos ---")
