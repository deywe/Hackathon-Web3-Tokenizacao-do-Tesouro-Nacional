// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ETI {

    struct Parlamentar {
        string nome;
        string estado;
        string municipio;
    }

    enum StatusAceitacao { Pendente, Aceito, Recusado }

    mapping(address => uint256) public valorTotalEmendas;
    mapping(address => Parlamentar) public perfisDeputados;
    mapping(address => mapping(address => StatusAceitacao)) public statusAceitacao;

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
        valorTotalEmendas[msg.sender] += valor;
        emit EmissaoTokens(msg.sender, valor);
    }

    function definirPerfilParlamentar(string memory nome, string memory estado, string memory municipio) public {
        Parlamentar storage parlamentar = perfisDeputados[msg.sender];
        parlamentar.nome = nome;
        parlamentar.estado = estado;
        parlamentar.municipio = municipio;
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
