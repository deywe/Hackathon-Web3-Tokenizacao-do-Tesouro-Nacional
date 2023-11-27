// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract EmendaToken is ERC20, Ownable {
    // Mapeamento para armazenar os saldos de tokens dos destinatários
    mapping(address => uint256) private _emendaBalances;

    // Evento emitido quando um destinatário recebe emendas
    event EmendaDistribuida(address indexed destinatario, uint256 valor);

    // Construtor do contrato
    constructor() ERC20("EmendaToken", "EMD") {
        _mint(msg.sender, 1000000 * 10**18); // Cria 1.000.000 de tokens e atribui ao criador do contrato
    }

    // Função para distribuir emendas a um destinatário específico
    function distribuirEmenda(address destinatario, uint256 valor) external onlyOwner {
        require(destinatario != address(0), "Endereço inválido");
        require(valor > 0, "O valor deve ser maior que zero");

        // Atualiza o saldo do destinatário
        _emendaBalances[destinatario] += valor;

        // Transfere os tokens para o destinatário
        _transfer(owner(), destinatario, valor);

        // Emite o evento de distribuição de emendas
        emit EmendaDistribuida(destinatario, valor);
    }

    // Função para consultar o saldo de emendas de um destinatário
    function saldoEmenda(address destinatario) external view returns (uint256) {
        return _emendaBalances[destinatario];
    }
}
