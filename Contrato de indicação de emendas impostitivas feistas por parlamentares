// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ETI {

    struct Parlamentar {
        string nome;
        string estado;
        string municipio;
        bool autorizado;
    }

    enum StatusAceitacao { Pendente, Aceito, Recusado }

    mapping(address => uint256) public valorTotalEmendas;
    mapping(address => Parlamentar) public perfisDeputados;
    mapping(address => mapping(address => StatusAceitacao)) public statusAceitacao;
    mapping(address => bool) public tokensAprovacao;

    event EmissaoTokens(address indexed parlamentar, uint256 valor);

    modifier apenasGoverno() {
        require(msg.sender == enderecoGoverno, "Apenas o governo pode executar esta função");
        _;
    }

    address enderecoGoverno;

    constructor() {
        enderecoGoverno = msg.sender;
    }

    function registrarValorTotalEmendas(uint256 valor) public apenasGoverno {
        valorTotalEmendas[msg.sender] = valor;
    }

    function emitirTokens(uint256 valor) public {
        require(tokensAprovacao[msg.sender], "Parlamentar não autorizado a emitir tokens");
        valorTotalEmendas[msg.sender] += valor;
        emit EmissaoTokens(msg.sender, valor);
    }

    function definirPerfilParlamentar(string memory nome, string memory estado, string memory municipio) public {
        Parlamentar storage parlamentar = perfisDeputados[msg.sender];
        parlamentar.nome = nome;
        parlamentar.estado = estado;
        parlamentar.municipio = municipio;
        parlamentar.autorizado = false;
    }

    function aprovarParlamentar(address parlamentar) public apenasGoverno {
        perfisDeputados[parlamentar].autorizado = true;
        tokensAprovacao[parlamentar] = true;
    }

    function enviarConviteAceitacao(address parlamentar, address prefeitura) public {
        require(tokensAprovacao[parlamentar], "Parlamentar não autorizado a enviar convites");
        require(statusAceitacao[parlamentar][prefeitura] == StatusAceitacao.Pendente, "Aceitação já processada");

        // Registre o convite de aceitação para a prefeitura
        statusAceitacao[parlamentar][prefeitura] = StatusAceitacao.Pendente;
    }

    function aceitarDestino(address parlamentar, address prefeitura) public apenasGoverno {
        require(statusAceitacao[parlamentar][prefeitura] == StatusAceitacao.Pendente, "Aceitação já processada");

        // Registre a aceitação da prefeitura
        statusAceitacao[parlamentar][prefeitura] = StatusAceitacao.Aceito;
    }

    function recusarDestino(address parlamentar, address prefeitura) public apenasGoverno {
        require(statusAceitacao[parlamentar][prefeitura] == StatusAceitacao.Pendente, "Aceitação já processada");

        // Registre a recusa da prefeitura
        statusAceitacao[parlamentar][prefeitura] = StatusAceitacao.Recusado;
    }

    function verificarStatusAceitacao(address parlamentar, address prefeitura) public view returns (StatusAceitacao) {
        return statusAceitacao[parlamentar][prefeitura];
    }
}
