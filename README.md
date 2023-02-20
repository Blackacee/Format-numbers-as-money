# Format-numbers-as-money

var num = 1234567.89,
 formatted;
formatted = num.toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,'); // "1,234,567.89"
More advanced variant with support of any number of decimals [0 .. n], variable size of number groups [0 .. x]
and different delimiter types:
/**
 * Number.prototype.format(n, x, s, c)
 *
 * @param integer n: length of decimal
 * @param integer x: length of whole part
 * @param mixed s: sections delimiter
 * @param mixed c: decimal delimiter
 */
Number.prototype.format = function(n, x, s, c) {
 var re = '\\d(?=(\\d{' + (x || 3) + '})+' + (n > 0 ? '\\D' : '$') + ')',
 num = this.toFixed(Math.max(0, ~~n));
 return (c ? num.replace('.', c) : num).replace(new RegExp(re, 'g'), '$&' + (s || ','));
};
12345678.9.format(2, 3, '.', ','); // "12.345.678,90"
123456.789.format(4, 4, ' ', ':'); // "12 3456:7890"
12345678.9.format(0, 3, '-'); // "12-345-679"
123456789..format(2); // "123,456,789.00"
